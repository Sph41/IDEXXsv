# Decision Brief: Day-7 Retention Recovery

*For: Marcus, Head of Product. Synthesized from [interview-synthesis.md](interview-synthesis.md), [nps-analysis.md](nps-analysis.md), and [competitive-matrix.md](competitive-matrix.md).*

*Note: a fourth planned source, `competitive-reddit.md`, does not exist in the workspace yet — this brief is built from three sources, not four, and does not include a Reddit-sentiment comparison.*

## Situation

Streakly's Day-7 retention dropped 9 points (48% → 39%) since the streak redesign shipped, concentrated in users who break their streak in week 1. The squad's discovery phase is confirming root cause before Thursday's meeting, where the goal is to align on the problem, not yet approve a solution.

## Key Findings

- **Streak breaks are the dominant driver of churn and complaints.** Missing two consecutive days in week 1 nearly doubles churn (squad data via interviews), and it's the single most repeated NPS theme — 6 of 10 raw comments, directly tied to reported uninstalls.
- **The failure moment compounds the damage on a second, separate axis.** Tom's interview and two independent NPS comments both flag the "you lost your streak" notification's tone and timing as its own failure point — distinct from the reset itself, not the same complaint restated.
- **Anxiety starts before any break — a post-break-only fix misses part of the week-1 population.** Amara (interviewed at day 4, no break yet) already dreads losing her streak, describing the app shifting from "game" to "chore." A recovery flow alone wouldn't reach users who disengage from anticipation, not failure.
- **Users are explicitly asking for what competitors don't offer.** NPS respondents want a "coach, not a scorekeeper." Competitive research shows even Duolingo — the only competitor with a real recovery mechanic (earnable Gems, app-initiated friend gifting, streak repair) — designs it around *preventing/undoing* loss, not acknowledging the return itself. No competitor owns that moment.
- **The mechanic has real upside worth protecting.** Priya's 14-month retention was cemented by the app celebrating her 30-day milestone — the loss-aversion/stakes design that hurts new users is the same thing that hooks long-term ones once the habit forms (~3 weeks, per her account).

## Options Considered

1. **Post-break "Comeback" screen only** (Lena's original concept — best-streak stat, comeback lesson, one-tap streak freeze). Addresses the failure moment directly but leaves pre-break anxiety (Amara-type users) untouched.
2. **Proactive early-signal only** — communicate forgiveness/low stakes to new users before day 4–5. Addresses anticipatory anxiety but does nothing for users who've already broken a streak.
3. **Combined direction** — pair a designed return-acknowledgment experience with proactive early signaling in week 1. Addresses both failure modes identified in the research and matches both competitive white-space gaps.

## Recommended Action

Approve a scoped design spike on Option 3 (combined direction), gated on validating both failure modes with real usage data before any build commitment.

## Why Now

The 9-point drop is already realized and measurable, three independent sources (interviews, NPS, competitive research) now converge on the same two-part failure mode, and at 28% YoY growth, every week without a fix compounds the number of new users lost to week-1 churn — Thursday's meeting is the natural point to move from discovery to a committed next step.
