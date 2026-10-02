# Stakeholder Profile: Raj

*Sources checked: [project.md](../01-orient/project.md), [strategy.md](../01-orient/strategy.md), [change_log.md](../01-orient/change_log.md), [CLAUDE.md](../CLAUDE.md), [decision-brief.md](../02-research/decision-brief.md), [nps-analysis.md](../02-research/nps-analysis.md), [hypothesis.md](../03-build/hypothesis.md), [triad-session.md](../03-build/triad-session.md), [codebase-summary.md](../04-team/codebase-summary.md). Note: [interview-synthesis.md](../02-research/interview-synthesis.md) does not mention Raj â€” it covers end-users (Priya/Tom/Amara), not squad stakeholders.*

## Role

**[Workspace-confirmed]** Originally framed as data/analytics: "Raj (data/analytics)" (CLAUDE.md, project.md), owning the churn/retention data. He's the one who surfaced that missing 2 consecutive days in week 1 nearly doubles churn.

**[Workspace-confirmed, later shift]** By the time of the triad-session prep, he's referred to as "Raj (Eng Lead)" / "Raj (eng)" â€” the person deciding technical feasibility and architecture. **Flag:** the workspace doesn't show an explicit transition between these two framings â€” worth confirming with Raj directly whether "eng lead" is his actual title or shorthand for "the technical decision-maker in the room."

**[Default profile]** Owns technical architecture, sprint scope, and feasibility decisions for the squad.

## Pushes Back On

**[Default profile]** Underspecified requirements, scope that grows mid-sprint, anything touching the streak/notification pipeline without a clear rollback plan.

**[Workspace-confirmed, consistent with above]** He already pushed back once, in substance: he flagged that "eligibility/targeting logic and streak-freeze rules are still undefined" for the original Comeback screen concept (project.md), even while confirming it was technically feasible with existing data. That's the same pattern the default profile describes â€” feasible in principle, but he won't sign off without the edge cases specified.

## Needs Before Saying Yes

**[Default profile]** Clear acceptance criteria, edge cases called out upfront, an answer to "what does done look like."

**[Workspace-confirmed]** Currently and specifically: an answer to where freeze-bank state lives â€” client-derived or server-authoritative â€” flagged as the one blocking technical question before a spec can be written ([triad-session.md](../03-build/triad-session.md)).

## Has Asked Before That I Struggled to Answer

**[Default profile]** "How will we know if this is working after it ships?" and "What happens if the user has never set a streak, or breaks it twice in a week?"

**Flag:** the second question is no longer unanswered â€” it's now the *central scenario* this entire project is built around. "Breaks a streak twice in a week" (missing 2 consecutive days in week 1) is the exact churn pattern from the original data, tested across three prototype personas, and the subject of [hypothesis.md](../03-build/hypothesis.md). If Raj asks this again, there's a real, evidenced answer now â€” this is worth leading with rather than treating as still-open.

## Communication Preference

**[Default profile]** Async-first, short messages, bullets over paragraphs, dislikes being surprised in standups. Not directly evidenced in the workspace â€” no communication-style data exists in any artifact.

## Open Items

**[Workspace-confirmed, matches default]** Still waiting on data-model clarification for the streak-freeze field. This is independently confirmed in [triad-session.md](../03-build/triad-session.md) as the explicit blocking question before spec-writing â€” the default profile's open item and the workspace's open item are the same thing, described from two different angles.

---

### What would most change how you prepare for Raj

His real open item (freeze-state ownership: client vs. server) has been flagged twice now â€” once in the triad-session prep, once in the default profile â€” and it's still unanswered. Don't walk into the next conversation with him without at least a proposed answer or a plan to get one (e.g., "here's the spike we'd run"); showing up empty-handed on the one thing he's already on record needing is the fastest way to not get a yes.
