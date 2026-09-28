# Streakly: Comeback Experience

## Overview

**Product:** Streakly — consumer habit + micro-learning app (5-min daily lesson, streak = core habit loop). 4 years old, Series B ($42M), 2.1M registered users, 340K MAU, +28% YoY.

**Squad:** Marcus (lead), Raj (data/analytics), Lena (user research/design), and me (PM).

**Current phase:** Discovery — confirming the root cause of the Day-7 retention drop before designing any solution. Not yet in solution design.

**Key stakeholders:** Marcus (squad lead, initiated the retention investigation), Raj (owns the churn/retention data), Lena (owns user research and the early Comeback screen concept).

## Planning Notes (PRD Skeleton)

*Source: #product-growth Slack thread, Monday 9:14am. Draft starting point, not final — to be aligned on before Thursday meeting.*

## Problem Statement

Day-7 retention dropped from 48% to 39% since the streak redesign shipped (Marcus). The drop is sharpest among users who break their streak in week 1 — once a user misses two days in a row, churn is almost double (Raj).

User research suggests the cause: when someone misses a day and their streak resets to zero, it feels like punishment rather than a normal setback, and there's no way back in (Lena). The app currently gives no acknowledgment of the break (same home screen, streak back at 0), and the "you lost your streak" push notification has a harsh tone that, when tapped, just drops users back at day zero with nothing offered (Lena). This combination is believed to cause users to go passive.

Working hypothesis (from thread): users disengage because breaking a streak feels like failure and there's no graceful comeback — recovery needs to feel specific to the user's own progress, not a generic "keep going!" message.

## Goals

- Understand and align on the underlying problem before designing solutions (explicit ask from Marcus for Thursday).
- Give users who break a streak a path back in that feels acknowledging rather than punishing.
- Address the tone/timing of the post-break notification, not just the in-app reset experience.

## Non-Goals

- Finalizing the specific comeback screen design (best-streak stat, 60-second comeback lesson, one-tap streak freeze) — these are Lena's early sketches, not agreed scope.
- Determining eligibility/targeting logic ("who sees it") and streak-freeze rules — Raj noted these still need to be defined, though no new data sources are required.
- Not otherwise specified in the thread.

## Success Metrics

- Day-7 retention (currently 39%, down from 48% pre-redesign) — implied metric to recover, no target stated in thread.
- Churn rate for users who miss two consecutive days in week 1 (currently ~2x baseline, per Raj) — implied secondary metric.
- No specific success targets or measurement window were defined in the thread; needs alignment before Thursday.
