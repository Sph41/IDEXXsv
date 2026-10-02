# Spec Readiness Review: Comeback Screen + Starter Freezes

*Post-Raj review sync. Date: 2026-10-01. Purpose: confirm technical architecture before writing formal spec.*

---

## 1. Spec Readiness Summary

| Component | Status | Blocker? | Notes |
|---|---|---|---|
| **Freeze-state ownership** | âœ… Resolved | No | Server-authoritative, persisted on `user.freezes_banked` |
| **Gap detection mechanism** | âœ… Resolved | No | Lazy check on app-open, derived from lesson log |
| **Freeze consumption logic** | âœ… Resolved | No | Retroactive 1-per-day, max 3, server-side decrement |
| **Freeze earning rules** | âœ… Resolved | No | `Math.min(3, floor(days_since_signup / 7))`, computed at app-open |
| **Timezone handling** | âœ… Resolved | No | User's local midnight = day boundary |
| **Lesson log schema & performance** | âš ï¸ **Spike required** | **Yes** | 2-hour spike to validate query perf + schema assumptions (blocks spec finalization) |
| **Notification integration** | âœ… Ready | No | Uses existing notification types; no new infra needed |
| **UI/interaction design** | ðŸŸ¡ Design-ready | No | Depends on Lena; Raj sees no technical blockers |

---

## 2. What Changed During Review

### Freeze-State Ownership (The Blocking Question)

**Originally:** Vague. PM said "server-side to allow switching between sites and apps," which Raj read as scope creep (cross-platform sync).

**Resolved:** Server-authoritative, user-level field (`user.freezes_banked`), but **not** for cross-platform sync â€” that's future scope. It's server-authoritative because:
- Prevents client/server drift
- Single source of truth for authorization (user can't lie about freeze count)
- Simpler than client-optimistic + reconciliation

**Impact on spec:** Adds one schema migration (add field to user model), no new API endpoints beyond what's already needed to fetch user state.

---

### Gap Detection & Freeze Consumption

**Originally:** "Use lesson log lazily, compute gap when they open the app, apply freezes 1 per day."

**Clarified in review:**
- **Gap definition:** Days since last completed lesson (derived from lesson log query on user.id, last_completed_date order)
- **Trigger:** App-open (lazy check, not a daily cron)
- **Consumption:** Retroactive, 1 freeze per day in the gap, up to the bank. If gap > freezes_banked, user still resets but gets friendly comeback screen.
- **Caching:** Cache the gap-recap (which days were saved vs. broken) with a 24h TTL; recompute if >24h old OR user just logged a new lesson
- **Performance:** Requires 2-hour spike to validate the lesson-log query doesn't become a bottleneck at scale

**Impact on spec:**
- No new endpoint; integrates into existing "fetch user state on app-open" flow
- New server-side logic: gap-detection function + retroactive freeze application
- New field: `user.last_gap_recap_calculated_at` (timestamp for cache invalidation)

---

### Freeze Earning

**Originally:** Not specified beyond "1 per 7 active days, cap 3" from hypothesis.md.

**Specified in review:**
- Formula: `freezes_banked = Math.min(3, floor((now - user.created_at) / 7 days))`
- Computed: At app-open, no daily cron needed (lazy, like gap detection)
- "Active" is implicit in the formula; no special activity tracking required

**Impact on spec:**
- No schema changes (uses existing `user.created_at`)
- No new cron job
- Two queries on app-open instead of one (gap check + freeze earning check), negligible cost

---

### Timezone Handling

**Originally:** Not mentioned.

**Specified in review:**
- Day boundary = user's local midnight, not server midnight
- "Gap days" counted using user's timezone
- Stored gap-recap includes user-local date labels

**Impact on spec:**
- All gap/freeze logic must pass user timezone to compute "today"
- Affects: gap-detection query, freeze-consumption loop, gap-recap UI rendering

---

## 3. What's Still Unresolved (& Why It's Okay)

| Item | Why it's okay | Next step |
|---|---|---|
| **Lesson-log schema details** | Will be discovered in 2-hour spike | Spike happens before spec is final |
| **Exact notification copy** | Design + content team decision | Call out in spec as "TBD pending design review" |
| **Comeback screen UI treatment** | Lena owns; prototype is proof-of-concept | Spec references prototype for interaction model; Lena finalizes visuals |
| **Reactivation metrics (Day-7+)** | Product/data decision; Raj doesn't own it | Flag in spec as "success metric TBD" |

---

## 4. Risk Flags for the Spec

**High confidence (2â€“3 week estimate holds if these are true):**
- Lesson log has reliable `(user_id, completed_date)` data and can be queried fast
- Notification system doesn't require refactoring to add "freeze_saved" or "comeback_available" events
- UI design from Lena doesn't require new client infrastructure

**Risk escalators (could add 1 week+):**
- Lesson-log schema is different than assumed (e.g., no per-lesson timestamps, or data is sparse/corrupted)
- Query performance is worse than expected after spike (would need caching layer / denormalization)
- Notification system is tightly coupled and needs refactoring
- Lena wants complex animations or real-time updates (would expand client work)

---

## 5. Architecture Decision: Rewrite Sections for the Spec

### A. Data Model Section (NEW)

```markdown
### Data Model

#### New Fields

- **user.freezes_banked** (Integer, min: 0, max: 3)
  - Count of available streak freezes, earned over time
  - Computed on app-open: `Math.min(3, floor(days_since_signup / 7))`
  - Decremented when retroactively applied to a missed-day gap
  - Server-authoritative; client reads this value, never increments it locally

- **user.last_gap_recap_calculated_at** (Timestamp, optional)
  - When the last gap-detection and recap-generation ran
  - Used to cache and avoid re-querying lesson log on every app-open
  - Invalidated if >24 hours old OR user logs a new lesson

#### Existing Fields (Used, not changed)

- **user.created_at** â€” used to compute `days_since_signup` for freeze earning
- **user.timezone** â€” used to compute day boundaries for gap detection
- **lesson_completions** table (or log) â€” query: `SELECT MAX(completed_at) WHERE user_id = ? AND completed_at < now()` to find last-completed date

#### Derived, Not Stored

- **current_gap** â€” computed on app-open: `days between user's last_completed_date and today (in user's timezone)`
- **gap_recap** â€” computed on app-open: array of day objects `[{date, saved_by_freeze: bool}, ...]` showing which missed days were covered
```

### B. Freeze Consumption Logic Section (REWRITE)

**Before (unclear):**
> "freeze consumption is lazy, 1 per day, when user returns"

**After (clear):**
```markdown
### Freeze Consumption: Retroactive Application on App-Open

When a user opens the app, the following happens:

1. **Gap Detection:** Query lesson_completions for the user's last completed date (in their timezone). Compute `gap_days = today - last_completed_date`.

2. **If gap_days = 0:** User is up to date. No action. Freeze count unchanged.

3. **If gap_days > 0:**
   - Compute `freezes_to_consume = min(gap_days, user.freezes_banked)`
   - Decrement `user.freezes_banked -= freezes_to_consume`
   - Generate gap_recap: array of `gap_days` entries, first `freezes_to_consume` marked `{saved_by_freeze: true}`, rest `{saved_by_freeze: false}`
   - If `freezes_to_consume < gap_days`: user's streak resets to 0; show friendly **Comeback Screen** with gap_recap
   - If `freezes_to_consume == gap_days`: streak is preserved; show **Saved-by-Freeze notification** with gap_recap
   - Set `user.last_gap_recap_calculated_at = now()`
   - Persist gap_recap for UI rendering (or cache it, implementation detail)

4. **Timezone:** All "today" and day boundary calculations use `user.timezone`, not server time.

5. **Idempotency:** Gap detection is idempotent. Calling it multiple times in the same calendar day (same user timezone) yields the same result. No freezes are consumed twice.
```

### C. Freeze Earning Section (UPDATED)

```markdown
### Freeze Earning: Points-Based, Displayed from Signup

Freezes are earned based on points accumulated by users, not tenure. All users understand the freeze mechanic from day 1.

**Freeze progression:**
- **Initial grant:** Every new user starts with 1 freeze at signup (no unlock needed)
- **Unlock #2:** When `user.total_points >= THRESHOLD_2` (TBD by PM; balance "not too easy" vs. "not too hard")
- **Unlock #3:** When `user.total_points >= THRESHOLD_3` (TBD by PM)
- **Cap:** 3 free freezes (beyond 3 is premium/paid plan feature)
- **Computation:** Freeze unlocks evaluated on app-open as part of the state-check; unlock events trigger notification/celebration
- **Monetization:** Free earned via points (no paywall for freezes #1â€“3); paid plan unlocks infinite/more freezes

**User communication (from signup):**
> "You start with 1 Streak Freeze. Unlock more as you earn points by completing lessons."

**Rationale:** Points-based earning ties freeze progression to active engagement (not just time passing). It creates a progression arc that feels earned, builds confidence in the app's generosity, and reduces perception of "free protection = low stakes." Important: confirm total points is reliably tracked and accessible for freeze-unlock logic.
```

---

## 6. Spike Plan (Blocking, 2 hours)

**Spike goal:** Validate that the lesson-log query is fast enough and schema assumptions are correct.

**Spike tasks:**
1. Query lesson_completions (or whatever the lesson-log table is called) for realistic data volume (1M+ users, 10M+ lesson entries)
2. Benchmark: `SELECT MAX(completed_at) WHERE user_id = ? AND completed_at < now()` â€” can it return in <50ms?
3. Confirm schema: does the table have indexed (user_id, completed_at)? If not, what's the cost of adding it?
4. Confirm data quality: are there any gaps, corrupted entries, or missing timestamps that would cause gap detection to fail?
5. Document findings in a spike summary; propose caching strategy if needed (e.g., denormalize `user.last_completed_date` on lesson insert)

**Owner:** Raj  
**Timeline:** Complete before spec is finalized; can happen in parallel with design iteration

---

## 7. Effort Estimate (Unlocked After Spike)

**Spike:** 2 engineer-hours  
**Feature (post-spike):** 2â€“3 weeks, one engineer

Breakdown:
- Schema migration + freeze-bank field: 1â€“2 points
- Point-based freeze unlock logic (on app-open, trigger notifications): 2â€“3 points
- App-open gap-detection + freeze-consumption logic: 5â€“8 points (core, highest risk)
- Lazy recap caching: 2â€“3 points
- Notifications (freeze-unlocked, freeze-saved, comeback-trigger): 3â€“4 points
- Testing (both directions: complete & uncomplete task, various gap scenarios, freeze unlocks): 4â€“6 points
- UI/design integration: depends on Lena's complexity; estimate TBD

**Risk escalators:**
- (+1 week) If spike reveals slow queries or data issues
- (+3â€“5 days) If notification system needs refactoring
- (âˆ’3â€“5 days) If UI is minimal (modal-only)

---

## 8. Ready for Spec? 

âœ… **Yes, pending spike.** Architecture is now clear. Raj has confidence in the 2â€“3 week estimate. 

**Spike outcome determines next step:**
- If spike is clean: Write spec immediately (Raj can own implementation with high confidence)
- If spike surfaces issues: Revise this spec section, adjust estimate, re-discuss with Marcus before kickoff

