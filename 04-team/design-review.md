# Design Review: Comeback Screen Feature

*Post-design-review sync with Lena. Date: 2026-10-02. Purpose: align on design strategy, scope, and iteration priorities.*

---

## Executive Summary

The prototype successfully addresses the emotional failure-mode problem flagged by Tom and Priya (harsh reset, no acknowledgment of what was built). The gap-recap visual (showing which missed days were saved vs. broken by freezes) is a strong differentiation from competitors. However, the feature doesn't yet address Amara's core problem: anticipatory anxiety *before* a break happens. That requires a separate design workstream (proactive onboarding communication). Within scope for this feature, copy and tone tuning is needed to ensure the comeback experience feels like support, not gamification, for anxious users.

---

## Design Review Matrix

| Component | What Works | What Needs Iteration | Owner | Priority |
|---|---|---|---|---|
| **Tone (notification + comeback heading)** | "Your streak is still waiting for you" â€” invitational, not punitive âœ… | "Comeback Bonus" XP badge still reads as achievement-chasing for Amara personas; need copy tuning | Lena | High |
| **Best-streak card** | Permanent record honored; "nobody can take it away" validates milestones âœ… | None identified | Lena | â€” |
| **Gap-recap rows** | Saved/broken visual clarity is strong; shows freeze logic granularly âœ… | Need to confirm users understand *why* certain days were saved; interactive explainer might be needed | Lena | Medium |
| **"Saved by freeze" screen** | Prevention scenario clear; shows the full value of freezes âœ… | Visual differentiation from Comeback screen not distinct enough; needs its own visual identity | Lena | High |
| **Lesson card + CTA** | Clear next action; "60-second comeback lesson" is concrete âœ… | None identified | Lena | â€” |
| **Freeze gift** | Gift positioned as welcome-back surprise âœ… | Copy ("we've got you covered") works, but needs to connect to onboarding communication so it feels expected, not surprising | Lena + PM | Medium |
| **Proactive "you have 2 freezes" communication** | Not in current proto âŒ | **OUT OF SCOPE for this feature.** Belongs in onboarding workstream; design will spec separately | Lena | Future sprint |

---

## Scope Decisions (Locked)

âœ… **In scope:**
- Copy tuning for anxious users (Amara persona)
- Full design of all three scenarios (under-7 break, 12-day break, saved-by-freeze)
- Gap-recap interaction/explainer design
- Visual differentiation between Comeback and Saved-by-Freeze screens
- Freeze mechanism clarity (how freezes work, why certain days were saved)

âŒ **Out of scope (separate workstreams):**
- Proactive freeze communication in onboarding
- Monetization/paywall design for freezes (product + growth decision)
- Broader gamification layer improvements (product initiative)

---

## Design Decisions & Rationale

### Decision 1: Amara-Focused Copy Tuning

**What:** Rewrite "Comeback Bonus XP" and achievement-framed language to emphasize support over accomplishment for anxious users.

**Why:** Persona testing flagged that Amara (new, anxiety-prone user) reads XP/bonus language as pressure, not encouragement â€” the opposite of the intended effect. Tom and Priya, by contrast, are motivated by achievement signals. Copy can be tuned to work for both audiences without removing mechanics.

**How:** 
- Replace "Comeback Bonus" with language like "Welcome back â€” day 1 is set" (no XP call-out in the copy, even though the backend rewards it)
- Reframe "locked in" language from achievement-tone ("you've achieved X") to stability-tone ("you're set up for tomorrow")
- A/B test messaging with anxious and motivated user cohorts post-launch

**Owner:** Lena  
**Timeline:** Finalize copy variants by end of this week for QA testing

---

### Decision 2: Visual Separation of "Saved by Freeze" from "Comeback"

**What:** Design distinct visual identity for the prevention scenario (user never actually broke the streak).

**Why:** These are two different moments (prevention vs. recovery) and feel emotionally different. Lumping them into the same visual template creates ambiguity: is the user celebrating a near-miss or recovering from an actual miss?

**How:**
- Saved-by-Freeze: shield/protection visual language (currently has ðŸ›¡ï¸ emoji, keep that) â€” emphasizes that the user was *protected*, not that they failed and recovered
- Comeback: acknowledgment visual language (currently has ðŸ”¥ emoji) â€” emphasizes "welcome back to your streak," not damage control
- Different color gradients or card treatments to reinforce the emotional distinction

**Owner:** Lena  
**Timeline:** Mockup variants by end of week; validate with Tom and Amara personas

---

### Decision 3: Freeze-Earning Mechanics & Communication

**What:** Users start with 1 freeze, unlock 2 and 3 as they accumulate points (not earned by tenure).

**Why:** Points-based earning ties freeze progression to active engagement, not just time. It creates a clear progression arc (1 â†’ 2 â†’ 3) and a monetization path (earn free via points, or buy infinite via paid plan).

**How:**
- **Upfront explanation:** Onboarding explains "You start with 1 Streak Freeze. Unlock more as you earn points by completing lessons." (Freezes are not hidden; users understand the mechanic from day 1)
- **Unlock moments:** Design needs to show when freeze #2 and #3 become available. Options:
  - In-app notification: "You've earned 500 points â€” unlock Freeze #2"
  - Progress bar in settings showing path to next freeze
  - Contextual nudge in the Comeback screen when user has banked freezes
- **Monetization callout:** When user maxes out free freezes (3), show option to buy more via paid plan (separate monetization workstream)
- **Point thresholds:** TBD by PM; need to balance "not too easy" vs. "not too hard" â€” probably 400â€“600 points for #2, 800â€“1200 for #3 (to be tested)

**Owner:** Lena (design), PM (point thresholds), Raj (tech feasibility)  
**Timeline:** Lena prototypes unlock flow by end of week; PM provides final point thresholds by Friday

---

### Decision 4: Ongoing Engagement Signals (Beyond Crisis Recovery)

**What:** Comeback calls and retention signals are not just reactive (after a break); design ongoing nudges to keep users on radar in a friendly way.

**Why:** The reactive Comeback screen only helps users who've already broken a streak. To increase Day-7 retention, we need proactive engagement signals that keep users thinking about the app between breaks.

**How:**
- **Examples** (design TBD, likely separate workstream): streak milestones ("You're on day 5!"), freeze unlock celebrations ("You've earned Freeze #2"), weekly check-ins ("Your streak is strong this week")
- **Tone:** Friendly, not pushy; celebration, not obligation
- **Scope:** Not part of this feature spec (Comeback screen is crisis recovery); belongs in broader retention/engagement workstream

**Owner:** Lena (design consultation), separate retention/comms team  
**Timeline:** Spike/plan in next sprint

---

### Decision 5: Gap-Recap Explainer (Interactive or Static)

**What:** Clarify why specific missed days were saved vs. broken by freezes.

**Why:** The gap-recap shows which days were covered, but users might not understand the *logic* (e.g., "I had 2 freezes banked, so days 1â€“2 were saved, day 3 wasn't"). Unexplained magic erodes trust.

**How:**
- Static copy option: "You'd banked 2 Streak Freezes by the time of your break. A freeze covers 1 day, so 2 of your 3 missed days were protected. The 3rd day reset your streak."
- Interactive option: Tap each gap-recap row to see a tooltip explaining the freeze logic for that specific day
- Decision point for Raj: does the client need to calculate and display which freezes were banked at the time of the break, or is server-side logic sufficient?

**Owner:** Lena (design), Raj (tech feasibility)  
**Timeline:** Prototype both by end of week; decide based on QA testing

---

## What User Research This Addresses

| Interview Finding | How Prototype Responds | Remaining Gap |
|---|---|---|
| **Tom: "All-or-nothing reset feels punishing"** | Best-streak card + gap-recap honor what was built; comeback tone is invitational not punitive âœ… | None â€” this is addressed |
| **Tom: "Harsh notification offered no path back"** | Notification tone + comeback screen provide a clear path (lesson, freeze, "not now") âœ… | None â€” this is addressed |
| **Priya: "Milestones create the hook"** | Best-streak display is prominent and permanent âœ… | None â€” this is addressed |
| **Amara: "Anxiety BEFORE any failure"** | Prototype is reactive (triggers after break); proactive "you have 2 freezes" communication is separate workstream âš ï¸ | **Proactive signaling needed in onboarding (out of scope for this feature)** |
| **Amara: "Pressure turns the product into a chore"** | Copy tuning in progress; XP/bonus language being revised to reduce achievement-feel âš ï¸ (in progress) | **Variants to be tested with Amara persona next week** |

---

## Highest-Impact Change for Week-1 Retention

**The Change:** Proactive "you have 2 free Streak Freezes" communication at signup/onboarding, shown *before* day 7.

**Why it matters:** Amara's core problem isn't recovering from a break â€” it's the anxiety *before* a break. She's already disengaging by day 4 because she dreads losing her progress. If she learns at signup that breaks are forgiven (up to 3 freezes), her anticipatory anxiety drops immediately, and she stays engaged through the critical first week.

**Business impact:** This is the actual Day-7 retention lever (per hypothesis.md). The Comeback screen helps reactivation (week-7+), but the proactive freeze communication prevents churn during the critical retention window (days 4â€“7).

**Scope note:** This is a **separate design workstream** from this feature, likely owned by Lena in collaboration with onboarding/signup flow. Not part of the Comeback screen feature spec, but critical for hitting the Day-7 retention target.

**Design owner:** Lena  
**Timeline:** Spike in next sprint; finalize by end of sprint 2

---

## Product Decision vs. Design Decision

### Product Decisions (PM owns)

- **When do freezes become visible?** âœ… LOCKED: At signup (not hidden). Users understand from day 1: "You start with 1 freeze, unlock more as you earn points."
- **How are freezes earned?** âœ… LOCKED: Points-based (1 at signup, unlock 2 and 3 at point thresholds). Not tied to tenure.
- **What are the point thresholds?** â³ TBD: PM to provide target points for unlock #2 and #3 (balance "not too easy" vs. "not too hard")
- **Monetization model?** âœ… LOCKED: Free earned via points (capped at 3); paid plan unlocks infinite/more freezes. Avoids gatekeeping core mechanic.
- **Freeze cap?** âœ… LOCKED: 3â€“5 is reasonable; beyond 5 suggests users should get a "pause" feature instead (future scope, not this sprint)
- **Is the Comeback screen shown for all breaks, or only breaks > 3 days?** (impacts frequency and user familiarity)
- **Ongoing engagement signals:** âœ… LOCKED: Not just reactive (crisis recovery); design ongoing nudges to keep users on radar in friendly way. Separate retention workstream.

### Design Decisions (Lena owns)

- **How should the Comeback and Saved-by-Freeze screens look visually different?**
- **What copy best conveys support (not achievement-chasing) for anxious users?**
- **Should the gap-recap have an interactive explainer, or is static copy sufficient?**
- **What visual language best communicates "you were protected by a freeze"?** (shield vs. other metaphor)
- **How should freezes be explained in onboarding?** (That's a separate workstream, but design owns the execution)

---

## Open Questions for Next Week

1. **PM to provide (blockers for tech spec):**
   - What are the point thresholds for freeze #2 and #3? (Need to balance "not too easy" vs. "not too hard")
   - When should unlock messages appear? (At milestone, in-app, in settings, or contextual?)
   - Is the Comeback screen shown for all breaks, or only breaks > 3 days?

2. **Lena's clarification points (waiting for PM answer):**
   - In "Saved by Freeze," do we show freeze history/bank, or just the celebration?
   - Gap-recap rows: interactive explainer or static copy?
   - How should the freeze-unlock progression be visualized in onboarding and settings?

3. **Raj's technical input needed:**
   - Can the gap-recap render which freezes were banked at the time of the break, or is that client-side or server-side?
   - Is there any performance concern with the gap-recap query/render on app-open?
   - How do we track point accumulation and trigger freeze unlocks? (Server-side validation?)

4. **Validation with users:**
   - Amara persona test on copy variants (achievement-tone vs. support-tone)
   - Tom persona test on gap-recap clarity (does he understand the freeze logic?)
   - Priya persona test on freeze-unlock messaging (does it feel rewarding, not grindy?)

---

## Next Steps

| Owner | Deliverable | Timeline | Blocker? |
|---|---|---|---|
| **PM** | **Finalize point thresholds for freeze #2 and #3** | **By Thursday** | **YES (blocks spec)** |
| **PM** | Clarify: Comeback screen shown for all breaks or only >3 days? | By Thursday | Yes (design scope) |
| **PM** | Clarify: When should freeze-unlock messages appear? | By Thursday | Yes (Lena's design) |
| Lena | Copy variants for Amara-focused tuning + visual mockups for Saved-by-Freeze differentiation | End of this week | |
| Lena | Gap-recap interactive vs. static prototype | End of this week | |
| Lena | Onboarding workstream spike plan (proactive freeze communication) | By Friday for sprint planning | |
| Lena | Freeze-unlock progression visualization (onboarding + settings) | End of this week | |
| Raj | Technical feasibility: point-based freeze unlocks (server-side validation) | By Friday | |
| Raj | Gap-recap query + performance validation | By Friday | |
| Team | Validation testing with Amara, Tom, and Priya personas on copy/visuals/unlock messaging | Next sprint | |

---

## Design Review Sign-Off

**Lena:** Design strategy locked. Ready to move to high-fidelity mockups pending clarifications above.

**PM:** Scope confirmed. Amara-focused copy tuning in scope. Proactive freeze communication moved to separate onboarding workstream.

**Raj:** Technical questions flagged; feedback due Friday.

