# Project Structure: Streakly Comeback Screen Feature

*Last updated: 2026-10-02. Tracks all folders, artifacts, and decision documents for the Comeback Screen initiative.*

---

## Directory Overview

```
IDEXXsv-main/
â”œâ”€â”€ 01-orient/                 # Problem definition & strategy
â”‚   â”œâ”€â”€ project.md            # Product overview, squad, goals, success metrics
â”‚   â”œâ”€â”€ strategy.md           # Working hypothesis on streak-break churn
â”‚   â””â”€â”€ change_log.md         # Discovery log, iterations, key findings
â”‚
â”œâ”€â”€ 02-research/              # User research & competitive analysis
â”‚   â”œâ”€â”€ interview-synthesis.md # 3 user interviews (Priya, Tom, Amara)
â”‚   â”œâ”€â”€ nps-analysis.md       # 10 raw NPS comments, top themes
â”‚   â”œâ”€â”€ competitive-matrix.md # 5 competitors (Duolingo, Babbel, etc.), white-space findings
â”‚   â””â”€â”€ decision-brief.md     # 1-page synthesis for Marcus (Head of Product)
â”‚
â”œâ”€â”€ 03-build/                 # Prototypes, specs, hypotheses
â”‚   â”œâ”€â”€ pm-brief.md          # Feature spec, persona, key decisions from interviews
â”‚   â”œâ”€â”€ hypothesis.md        # Learning synthesis: known, assumed, unknown
â”‚   â”œâ”€â”€ triad-session.md     # 30-min working session agenda (Raj + Lena)
â”‚   â””â”€â”€ prototype/
â”‚       â”œâ”€â”€ index.html       # Clickable prototype: 375px phone mockup, 3 scenarios
â”‚       â””â”€â”€ README.md        # Prototype documentation, design decisions
â”‚
â”œâ”€â”€ 04-team/                  # Technical & design collaboration docs
â”‚   â”œâ”€â”€ collaboration.md      # Team working template (Codebase Tour, Design Review, QA)
â”‚   â”œâ”€â”€ codebase-summary.md   # PM-level Habitica codebase tour (reference research)
â”‚   â”œâ”€â”€ spec-readiness.md     # Technical architecture locked by Raj (data model, freeze earning, gap detection)
â”‚   â”œâ”€â”€ design-review.md      # Design decisions locked by Lena (tone, visuals, scope)
â”‚   â””â”€â”€ qa-checklist.md       # Edge cases matrix + PM QA scorecard + PR comment template
â”‚
â”œâ”€â”€ stakeholders/             # Individual stakeholder profiles & context
â”‚   â”œâ”€â”€ raj.md               # Engineering Lead: needs, pushbacks, open items
â”‚   â”œâ”€â”€ lena.md              # Designer: needs, pushbacks, open items
â”‚   â””â”€â”€ marcus.md            # Head of Product: needs, pushbacks, open items
â”‚
â”œâ”€â”€ CLAUDE.md                 # Persistent memory: product overview, squad, role
â”œâ”€â”€ MEMORY.md                 # Index of auto-memory files (if using memory system)
â”œâ”€â”€ STRUCTURE.md              # This file â€” directory guide
â””â”€â”€ README.md                 # (if exists) Project root documentation
```

---

## Key Artifacts by Phase

### Phase 1: Orient (Problem Definition)
- **[01-orient/project.md](01-orient/project.md)** â€” Product overview, squad, current problem (Day-7 retention dropped 9 pts)
- **[01-orient/strategy.md](01-orient/strategy.md)** â€” Root cause hypothesis: streak-break is punishing, no way back in
- **[01-orient/change_log.md](01-orient/change_log.md)** â€” Discovery iterations, key findings log

### Phase 2: Research (Evidence Gathering)
- **[02-research/interview-synthesis.md](02-research/interview-synthesis.md)** â€” 3 user personas (Priya, Tom, Amara) with direct quotes
- **[02-research/nps-analysis.md](02-research/nps-analysis.md)** â€” 10 NPS comments, top themes, actionable issues
- **[02-research/competitive-matrix.md](02-research/competitive-matrix.md)** â€” Competitor research (Duolingo, Babbel, Elevate, Memrise, Brilliant)
- **[02-research/decision-brief.md](02-research/decision-brief.md)** â€” 1-page brief for Marcus: Situation â†’ Findings â†’ Options â†’ Recommendation

### Phase 3: Build (Prototype & Specification)
- **[03-build/pm-brief.md](03-build/pm-brief.md)** â€” Feature spec, persona, key decisions from interviews
- **[03-build/hypothesis.md](03-build/hypothesis.md)** â€” Learning synthesis, metrics, known vs. assumed vs. unknown
- **[03-build/triad-session.md](03-build/triad-session.md)** â€” Working session agenda with Raj (eng) + Lena (design), 5 decisions to nail
- **[03-build/prototype/index.html](03-build/prototype/index.html)** â€” Interactive prototype: 3 scenarios (under-7, 12-day, saved-by-freeze)
- **[03-build/prototype/README.md](03-build/prototype/README.md)** â€” Prototype documentation, out-of-scope items

### Phase 4: Team (Technical & Design Alignment)
- **[04-team/collaboration.md](04-team/collaboration.md)** â€” Template for Codebase Tour, Design Review, QA
- **[04-team/codebase-summary.md](04-team/codebase-summary.md)** â€” PM-level tour of Habitica codebase (reference research, not Streakly)
- **[04-team/spec-readiness.md](04-team/spec-readiness.md)** â€” âœ… **LOCKED** Technical architecture (Raj sign-off)
  - Data model: server-authoritative freeze-bank field
  - Freeze earning: points-based (1 at signup, unlock 2â€“3 via point thresholds TBD)
  - Gap detection: lazy check on app-open, retroactive freeze application
  - Blocking spike: 2-hour lesson-log query performance validation
- **[04-team/design-review.md](04-team/design-review.md)** â€” âœ… **LOCKED** Design decisions (Lena sign-off)
  - Amara-focused copy tuning (XP badge pressure)
  - Visual differentiation: Saved-by-Freeze vs. Comeback screens
  - Scope: freeze mechanics in this feature; proactive onboarding in separate workstream
- **[04-team/qa-checklist.md](04-team/qa-checklist.md)** â€” Edge cases (21 test cases), PM QA scorecard (10 requirements), PR comment template

### Stakeholders
- **[stakeholders/raj.md](stakeholders/raj.md)** â€” Engineering Lead profile: needs, pushbacks, open items (freeze-state ownership)
- **[stakeholders/lena.md](stakeholders/lena.md)** â€” Designer profile: needs, pushbacks, open items (XP tone tuning)
- **[stakeholders/marcus.md](stakeholders/marcus.md)** â€” Head of Product profile: needs, pushbacks, open items (rollout timeline)

---

## Workflow & Status

### Current Phase: **QA & Implementation**
- âœ… Problem defined and validated through interviews
- âœ… Research completed (interviews, NPS, competitive analysis)
- âœ… Prototype built and iterated (3 rounds of persona testing)
- âœ… Technical architecture locked (Raj sign-off on spec-readiness.md)
- âœ… Design decisions locked (Lena sign-off on design-review.md)
- âœ… QA edge cases identified and PM checklist created
- â³ **Pending:** 2-hour spike on lesson-log query performance (Raj)
- â³ **Pending:** Point thresholds for freeze #2 and #3 (PM to provide)
- â³ **Pending:** Implementation by Raj (2â€“3 weeks after spike)

### Next Steps (By Owner)
| Owner | Deliverable | Timeline | Blocker? |
|---|---|---|---|
| PM | Freeze earning point thresholds (#2, #3) | By Thu, Oct 3 | Yes |
| Raj | Lesson-log spike (query perf + schema validation) | By end of week | Yes |
| Lena | Copy variants (Amara persona tuning) + visual mockups (Saved-by-Freeze differentiation) | By end of week | No |
| Lena | Freeze-unlock progression visualization (onboarding + settings) | By end of week | No |
| Team | Persona validation testing (Amara, Tom, Priya) | Next sprint | No |

---

## How to Use This Structure

**For new collaborators:**
1. Start with [CLAUDE.md](CLAUDE.md) for product context
2. Read [01-orient/project.md](01-orient/project.md) for the problem
3. Skim [02-research/decision-brief.md](02-research/decision-brief.md) for the recommendation
4. Deep dive: [04-team/spec-readiness.md](04-team/spec-readiness.md) (tech) + [04-team/design-review.md](04-team/design-review.md) (design)

**For decision reviews:**
- Understand the *why*: [01-orient/strategy.md](01-orient/strategy.md) + [02-research/interview-synthesis.md](02-research/interview-synthesis.md)
- Review the *what*: [03-build/hypothesis.md](03-build/hypothesis.md) + [03-build/prototype/](03-build/prototype/)
- Validate the *how*: [04-team/spec-readiness.md](04-team/spec-readiness.md) + [04-team/design-review.md](04-team/design-review.md)

**For QA & testing:**
- See [04-team/qa-checklist.md](04-team/qa-checklist.md) for edge cases and sign-off criteria

**For stakeholder alignment:**
- Individual profiles: [stakeholders/raj.md](stakeholders/raj.md), [stakeholders/lena.md](stakeholders/lena.md), [stakeholders/marcus.md](stakeholders/marcus.md)

---

## Open Decisions & Blockers

| Decision | Owner | Status | Impact |
|---|---|---|---|
| Point thresholds for freeze #2 and #3 | PM | â³ TBD | Blocks tech spec |
| Freeze cap: strictly 3 or up to 5? | PM | âœ… Locked: 3â€“5 reasonable, >5 suggests pause feature | Tech scope |
| Comeback shown for all breaks or only >3 days? | PM | â³ TBD | Design scope |
| Lesson-log query performance acceptable? | Raj (spike) | â³ Pending 2h spike | Tech risk |
| Amara copy tuning (XP badge pressure)? | Lena | âš ï¸ In progress | Design iteration |
| Proactive freeze communication in onboarding? | Lena | ðŸŸ¡ Separate workstream (sprint 2) | Product strategy |

---

## Version History

| Date | Phase | Changes |
|---|---|---|
| 2026-09-30 | Research | Added codebase-summary.md (Habitica reference research) |
| 2026-10-01 | Collaboration | Added spec-readiness.md (Raj review), design-review.md (Lena review), stakeholder profiles |
| 2026-10-02 | QA | Added qa-checklist.md (21 edge cases, PM scorecard, PR comment template) |
| 2026-10-02 | Sync | Added STRUCTURE.md (this file) â€” directory guide for all artifacts |

