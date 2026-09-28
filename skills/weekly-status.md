---
name: weekly-status
description: Turn raw bullet-point notes into a formatted leadership status update with Shipped, In Progress, Blockers, and Next Week sections. Use when asked to write, draft, or format a weekly status update, leadership update, or standup summary from rough notes.
---

# Weekly Status Update

Converts raw, unordered bullet notes into a leadership-ready status update.

## Input

Raw bullet-point notes — unordered, informal, may mix completed work, ongoing work, problems, and plans in any order.

## Output

A formatted update with exactly these four sections, in this order:

1. **Shipped**
2. **In Progress**
3. **Blockers**
4. **Next Week**

## Rules

- Maximum 3 bullets per section. If more than 3 items belong in a section, keep the 3 most significant and drop or merge the rest — do not add a 4th bullet or a sub-list to work around the limit.
- If a section has no items, write "None" rather than omitting the heading.
- Use plain, declarative language. No jargon, no buzzwords, no hedging ("we're hoping to," "should probably"). State what happened or will happen.
- Each bullet is one sentence, stating the fact plainly (what shipped, what's in progress, what's blocking, what's next) — not a narrative or justification.
- Do not invent information not present in the input notes. If a note is ambiguous about which section it belongs in, use judgment based on its content (e.g., "waiting on X team" → Blockers) but don't fabricate details to fill it out.

## Example

**Input notes:**
- finished the onboarding redesign, launched Tuesday
- still working on the billing migration, about 60% done
- waiting on legal review for the new ToS before we can ship the signup flow
- need to start planning Q3 roadmap next week
- fixed the login bug from last sprint
- exploring a partnership with Acme, too early to report

**Output:**

**Shipped**
- Launched the onboarding redesign on Tuesday.
- Fixed the login bug from last sprint.

**In Progress**
- Billing migration is about 60% complete.

**Blockers**
- Signup flow is blocked on legal review of the new ToS.

**Next Week**
- Start planning the Q3 roadmap.
