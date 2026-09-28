# Strategy: Recovering the Day-7 Retention Drop

## The Gap

Day-7 retention fell 9 points, from 48% to 39%, since the streak redesign shipped.

## Working Hypothesis

Users disengage after breaking a streak because the experience feels like punishment with no way back in — not because the underlying lesson content or product value changed.

Supporting signal (from squad discussion, not yet validated as root cause):
- The drop is sharpest among users who break their streak in week 1.
- Missing two consecutive days roughly doubles churn.
- After a break, the app gives no acknowledgment (streak silently resets to 0, same home screen) and sends a harshly-toned "you lost your streak" notification that leads nowhere.

## Status

This is a hypothesis, not a confirmed root cause. Current phase priority is to validate it (or rule it out) before committing to a solution direction — per the squad's alignment that Thursday's discussion is about the problem, not the fix.

## Candidate Direction (not committed)

Lena has sketched an early "Comeback screen" concept (best-streak stat, a 60-second comeback lesson, one-tap streak freeze) as one possible response if the hypothesis holds. Raj confirmed it's technically feasible with existing data sources. This is not yet approved scope — targeting/eligibility logic and streak-freeze rules are still undefined.
