# QA Checklist: Comeback Screen Feature

*Comprehensive edge case testing + PM sign-off verification. Feature heading to Raj for implementation.*

---

## 1. Comprehensive Edge Case Matrix

### A. Empty States (First-Time User Scenarios)

| Case | Description | Expected Behavior | Priority | Risk |
|---|---|---|---|---|
| **Never broke before** | User opens app for first time after signup, no streak yet | Comeback screen should NOT show; direct to home/lesson | High | High â€” false positive would confuse new users |
| **No best-streak stat** | User broke streak on day 1 (best streak = 0 or null) | Best-streak card still renders; shows "0 days" or "Your first attempt" (copy TBD) | Medium | Medium â€” should be graceful, not crash |
| **No lessons available** | User's track has no lessons (lesson library empty) | "60-second comeback lesson" card is hidden or shows "lesson unavailable" CTA instead | Medium | Medium â€” degrades gracefully but should be noted |
| **No track assigned** | User has no active track selected | Comeback screen shows generic "Continue your streak" instead of track name | Medium | Low â€” rare, but should be handled |
| **First-time streak break** | User on day 3, breaks streak, first time seeing Comeback screen | Should feel supportive, not like a failure; copy emphasizes "life happens" not "you failed" | High | High â€” tone is critical to Amara persona |

---

### B. Edge Data Conditions (Freeze & Streak Logic)

| Case | Description | Expected Behavior | Priority | Risk |
|---|---|---|---|---|
| **Streak of 1** | User breaks on day 2 (best streak = 1) | Best-streak card shows "1 day" without feeling embarrassing; confirmation shows "1 day is your record" | Medium | Low â€” edge case but straightforward |
| **Broke twice in a week** | User breaks on day 2, gets freeze gift, breaks again day 8 | Each break shows Comeback screen + another freeze gift; freezes can stack up to max (3?) | High | High â€” freeze limit logic must be correct |
| **Freeze already used** | User returned after first break, used their gifted freeze, now breaks again | Gap-recap should show which days were covered by freeze #1, new freeze is offered | High | High â€” gap-recap must be clear about which freeze covered which days |
| **Multiple freezes banked** | User with 2 earned freezes (from ongoing earn) breaks after 3-day gap | Gap-recap shows 2 days saved (by 2 freezes), 1 day unprotected; streak resets but partially covered | High | High â€” gap-recap logic is core to this feature |
| **Freeze cap reached** | User has 3 freezes, breaks streak, uses all 3 in the gap, gets another gift | Should not exceed freeze cap (unless that's the monetization path); or offer premium freeze | Medium | Medium â€” monetization decision, not a bug |

---

### C. Timing Scenarios (Temporal Edge Cases)

| Case | Description | Expected Behavior | Priority | Risk |
|---|---|---|---|---|
| **Missed one day** | User missed 1 day, one freeze covers it, streak never resets | Show "Saved by freeze" screen (not Comeback); celebrate prevention | High | High â€” must distinguish prevention from recovery |
| **Missed many days** | User missed 7+ days, has 2 freezes, 5 days unprotected | Gap-recap shows 2 saved, 5 broken; streak resets; fresh start | High | Medium â€” gap-recap scales but should be tested with large gaps |
| **Timezone boundary miss** | User in PST, missed "yesterday" but timezone makes it ambiguous | Gap calculation uses user's local midnight, not server time; must be consistent | High | High â€” timezone bugs are silent and hard to catch |
| **User offline at break time** | User misses a day while offline, opens app 2 days later | Gap is correctly calculated as 2 days (not 1), freezes applied retroactively | High | High â€” offline state could confuse gap detection |
| **Comeback shown too late** | User breaks, doesn't open app for 30 days, finally returns | Comeback screen still shows (or does it disappear after N days?); best-streak is unchanged | Medium | Low â€” grace period for showing comeback is a product decision, not a bug |
| **User checks in exactly at midnight** | User completes lesson at 11:59 PM, checks app again at 12:01 AM (next day) | Lesson is counted as today; next opening after that is day 1, not a break | Medium | Low â€” timing is precise but should be tested |

---

### D. Permission & Notification States

| Case | Description | Expected Behavior | Priority | Risk |
|---|---|---|---|---|
| **Notifications off** | User disabled notifications, streak breaks | Comeback screen still shows when user opens app (not dependent on notifications) | High | Medium â€” feature doesn't break, but user might not know to open app |
| **Background refresh off** | User disabled background app refresh, breaks streak | Gap-detection happens on app-open (not in background), so still works | High | Medium â€” same as above; no silent breakage |
| **App uninstalled, reinstalled** | User's device wiped, app reinstalled, logs back in | All streaks and best-streak stats restored; if user broke before uninstall, Comeback screen shows on re-login | High | High â€” authentication + data recovery must be seamless |
| **Multiple devices** | User breaks on phone, opens app on tablet (same account) | Same account sees same streak state; gap-recap is consistent across devices | Medium | High â€” if data is device-specific, this breaks sync |

---

### E. Copy & Tone Edge Cases (Messaging Tone)

| Case | Description | Expected Behavior | Priority | Risk |
|---|---|---|---|---|
| **Amara persona (anxious, new user)** | User on day 4, breaks, sees Comeback screen | Copy emphasizes "life happens, no pressure" not "achievement" or "failure" | High | High â€” tone can flip Amara from churn to retention |
| **Tom persona (experienced, high-streak break)** | User with 12+ day streak breaks, sees Comeback screen | Best-streak honored prominently; copy acknowledges what was built | High | High â€” tone must validate long streaks without feeling patronizing |
| **Priya persona (power user, hitting milestones)** | User with multiple comebacks; frozen freeze gift on 3rd+ break | Should not feel repetitive or patronizing; copy adapts or fades | Medium | Low â€” can be addressed post-launch if needed |

---

## 2. PM QA Checklist vs. Prototype

**Evaluating prototype against 10 critical PM requirements:**

| # | Requirement | Prototype Status | Pass/Fail | Notes |
|---|---|---|---|---|
| **1** | **Best-streak is never zeroed** | âœ… Under-7-day persona shows "6 days" in permanent card; 12-day shows "12 days" | **PASS** | Best-streak card is visually prominent; copy: "that's yours for good" reinforces permanence |
| **2** | **Comeback screen tone is supportive, not punitive** | âœ… Copy: "Life happens. Missing a couple of days doesn't erase what you built" | **PASS** | Tone is explicitly forgiving; no shame language |
| **3** | **Gap-recap shows which days were saved vs. broken** | âœ… Renders saved (green/freeze icon) vs. broken (gray/empty) rows for each missed day | **PASS** | Visual distinction is clear; saved rows show "ðŸ§Š Saved automatically by a Streak Freeze you'd already banked" |
| **4** | **Freeze gift is offered, not forced** | âœ… Card with CTA button "Add Streak Freeze to my bank" + "Not now" link | **PASS** | User agency is preserved; accepting/declining changes confirmation copy |
| **5** | **Comeback lesson is clear and low-friction** | âœ… "60-second comeback lesson" with track name + CTA "Start comeback lesson" | **PASS** | Concrete, scoped; lesson shown in proto with actual content ("Quick refresher: open chords") |
| **6** | **Saved-by-freeze scenario feels like prevention, not recovery** | âœ… Separate screen for saved-by-freeze with shield emoji ðŸ›¡ï¸ + "Your streak is safe" | **PASS** | Visual + messaging differentiate from Comeback (recovery) scenario |
| **7** | **Notification on lock screen invites, not shames** | âœ… Notification: "Your streak is still waiting for you. Jump back in whenever you're ready ðŸ‘‹" | **PASS** | Tone is invitational, not punitive |
| **8** | **Feature handles both under-7-day AND longer-streak breaks** | âœ… Two personas (Under-7-day, 12-day) demonstrate range; same screens, different numbers | **PASS** | Scalable; copy and numbers adjust, mechanic is the same |
| **9** | **Freeze mechanics are understandable to new users** | âš ï¸ Explanation in gap-recap is clear, but onboarding communication is NOT in prototype | **PARTIAL** | Prototype assumes user already understands freezes; onboarding explainer is a separate workstream (noted in design-review.md) |
| **10** | **XP/gamification doesn't feel like pressure to anxious users** | âš ï¸ Prototype includes "â­ +15 XP Comeback Bonus" badge | **PARTIAL FAIL** | Persona testing (Amara) flagged this as MORE pressure, not less; copy tuning is in progress (design-review.md Decision #1) |

**Blockers for launch:** None (partial issues are design/onboarding, not feature blockers)  
**Known issues to address post-QA:** Copy tuning for Amara persona (#10); onboarding explainer for freeze mechanics (#9)

---

## 3. PM Comment for Raj's PR

*When Raj opens the PR to merge the feature:*

---

**PM Review: Comeback Screen Implementation**

One question before merge:

**When a user misses a day while offline (no background refresh), and then opens the app 2+ days later â€” how does the gap-detection handle the state transition?**

Specifically: Let's say user is day 5, misses day 6 (offline), opens app on day 8. The gap is 2 days. But when we run gap-detection on app-open:

1. Do we check `last_completed_lesson < (now - 1 day)` and immediately consume up to 2 freezes retroactively?
2. Or do we check `lesson history` and reconstruct which specific days were missed, then apply freezes per-day in order?
3. If the user has 1 freeze banked, does it cover day 6 (the first miss) or day 7 (the second)?

The reason I'm asking: the gap-recap UI shows which specific days were "saved" vs. "broken," so we need the backend logic to match what the UI is promising. If a user sees "Day 6 was saved, Day 7 was broken" but actually had a 1-freeze bank, that's confusing.

From the spec (spec-readiness.md), it looks like we're applying freezes 1-per-day in order, but I want to confirm the offline scenario is covered â€” especially since offline + break + delayed re-opening is a real edge case for mobile apps.

Should we add a test case for this, or is it already covered in the spike?

---

**Summary:** This is a good-faith clarification, not a blocker. The logic is sound in the spec, but "offline user with delayed gap detection" deserves explicit coverage.

---

## 4. Risk Summary Before Sign-Off

| Risk | Severity | Owner | Mitigation |
|---|---|---|---|
| Gap-recap timezone logic (user midnight vs. server midnight) | **High** | Raj | Test with users across multiple timezones; add explicit timezone test to suite |
| Freeze cap / monetization boundary | **Medium** | PM/Raj | Clarify: what happens when user reaches 3 free freezes and breaks again? |
| Copy tuning for Amara (XP badge pressure) | **Medium** | Lena/PM | A/B test variants post-launch; can be hotfixed if needed |
| Onboarding communication (freezes explained upfront) | **Medium** | Lena | Out of scope for this sprint, but critical for Day-7 retention; spike in sprint 2 |
| Device sync consistency (multi-device users) | **Medium** | Raj | Audit: is streak state server-authoritative, or can devices diverge? |

