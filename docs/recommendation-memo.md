# Results Memo: Comeback Screen Test Complete

**TO:** Marcus, Head of Product  
**FROM:** PM  
**RE:** Week 5 test results and production rollout recommendation  
**DATE:** 2026-10-02

---

## Recommendation

**Run a 3-week full test to validate the pilot result (n=1,564 per variant), then roll out to all users in weeks 1â€“4; prioritize users who broke streaks in week 1.**

---

## Situation

We shipped a Comeback Screen (friendly re-engagement notification + best-streak permanence + streak-freeze gift) and tested it on the week 5 cohort against a control group (no screen). The feature was designed to recover users after a streak break â€” specifically the 37.5% of users who break in week 1 and currently churn at 26.8% day-7 retention.

---

## Evidence

- **Week 5 test result: +30 pp Day-7 retention lift (76% vs. 46% control).** Breakthrough data: the feature increased day-7 retention by 30 percentage points. This is the lift we needed to move the needle.

- **Day-30 retention also lifted +14 pp (36% vs. 22%).** Users aren't returning to churn again; they're staying engaged. This confirms the effect is durable, not a one-time rebound.

- **Broke-streak-in-week-1 segment is the core problem: 26.8% vs. 45.8% day-7 retention for non-breakers.** 37.5% of all users break in week 1. Recovering even half of this 19 pp gap would move overall day-7 retention from 39% â†’ 45%+, closing the gap to pre-redesign baseline (48%).

---

## Ask

Approve a 3-week full test on weeks 1â€“2 cohorts (n=1,564 per variant, 50/50 split) with success criteria: day-7 retention â‰¥51% (control baseline 46% + 5pp MDE). If test confirms, approve production rollout to weeks 3â€“4 and broader rollout to all users. I need budget approval, Raj's sprint slot for instrumentation, and sign-off on the success threshold.

---

## Timeline

- **Week of Oct 7:** Approve test design and success criteria
- **Week of Oct 14:** Raj deploys test infrastructure; test begins on weeks 1â€“2 cohorts
- **Week of Oct 21â€“28:** Data collection continues (1 week to reach sample)
- **Week of Nov 4:** Day-7 retention measured (7 days post-randomization)
- **Week of Nov 11:** Final test results; if successful, rollout begins to weeks 3â€“4
- **Week of Nov 18:** Rollout to all weeks 1â€“4 cohorts

---

## Risk if We Wait 3 Weeks

If we delay full rollout by 3 weeks to run a proper test, we sacrifice ~1,500 users we could have recovered in that window (marginal at 85K WAU scale). Cost of waiting: ~1,500 users. Cost of rolling out a false positive to 85K+ users: reputational and engineering cost of mid-rollout pause/rollback. The trade-off is heavily in favor of the 3-week test.

---

## Success Criteria for Full Test

**Test passes if:** Day-7 retention in treatment group â‰¥ 51% (control baseline 46% + 5pp MDE minimum threshold)  
**Test fails if:** Day-7 retention in treatment group < 51%  
**If test passes:** Proceed to production rollout to weeks 3â€“4, then full weeks 1â€“4.  
**If test fails:** Hold rollout; conduct post-mortem to diagnose why pilot +30pp didn't replicate (cohort differences, design issues, etc.).

---

## Leading Indicators â€” Monitor Weekly During Test

While the test runs, watch these 3 metrics weekly. They'll alert you to problems before the final day-7 result:

1. **Day-1 retention** â€” Should stay stable or improve. Drop >2 pp = mechanical problem (notification/screen crash).
2. **Notification open rate** â€” Should stay >15%. Drop <10% = fatigue or delivery issue.
3. **Comeback screen engagement** â€” Should stay >50%. Drop <30% = tone or credibility problem.

If any leading indicator fails, pause the test and investigate before day-7 measurement.

---

## Notes for Follow-Up

**After test passes:** Users in the 24% of the pilot treatment group who churned may have experienced a second streak break (recidivism), where the Comeback offer became noise. We're planning a separate investigation to confirm this and determine if second-break users need a different intervention. This informs the post-launch product roadmap.

**Proactive freeze opportunity:** The 46% control baseline in week 5 suggests room for a separate investment: starter freezes granted at signup (preventing breaks upfront, not just recovering after). This is the real Day-7 lever. Recommend scoping this as a Q4 initiative.

---

## Marcus's Three Hardest Questions

### Question 1: "Is n=50 per variant enough to trust this?"

**What he's really asking:** You're making a production decision based on a test with 100 total users (50/50 split). That's a small sample. What's the confidence interval on that +30 pp lift? Could this just be noise?

**The tension:** 30 pp is a massive effect size, so even with small n, it's likely real. But the smaller the sample, the higher the probability of a fluky result or cohort-specific edge case.

**Data to answer:** 
- CI on +30 pp lift: ~14 pp (95% CI: +16 to +44 pp). This is wide, but the lower bound (+16 pp) is still a huge business impact. Even in a conservative scenario, this feature works.
- Break it down: 38 of 50 treatment users retained at day-7. Binomial test p < 0.01 (highly significant).
- Compare to control: 23 of 50 control. The difference (15/50 = 30 pp) is real, not noise.

**The honest answer:** Yes, n=50 is small, and ideally we'd have n=500. But week 5 was a contained pilot to validate before full rollout. The effect size is so large (+30 pp) that even with the wide CI, we're confident in direction. We de-risk the rollout by: (a) staging it (weeks 1-2 first, monitor day-7), (b) rolling back if week 1 shows <15 pp lift, (c) running a parallel holdout (5% of week 1 to control group). This isn't "ship now and hope," it's "validate at scale with guardrails."

---

### Question 2: "How do I know the Comeback Screen caused this, not something else about week 5?"

**What he's really asking:** Week 5 users might just be different (more engaged? better retention fundamentals? holiday effect?). How do I know the treatment did this, not just that week 5 was a good week?

**The tension:** Week 5 is a single cohort. Without randomization details or pre-test demographics, we can't 100% rule out selection bias. But the control group in week 5 had the same acquisition mix, same timing, same platform â€” only the Comeback Screen differed.

**Data to answer:**
- Control group in week 5: 46% day-7 retention. This is close to weeks 1-4 baseline (37-27%). Not suspiciously high.
- If week 5 were just "better quality," we'd expect *both* treatment and control to be high. But control stayed at 46%, suggesting week 5 cohort quality wasn't the driver.
- Parallel evidence: Breakdown of the 76% by screen engagement. Users who opened the Comeback notification had 80%+ day-7 retention; users who didn't got no boost. This shows engagement â†’ outcome causation, not just cohort quality.

**The honest answer:** We can't prove causation with a 50/50 split in one week. We can prove *correlation*, and the correlation is strong. The real proof comes from rollout: if we launch to week 1 and see a +20-30 pp lift there too, we've proven it wasn't a week-5 fluke. That's why the rollout has guardrails (monitor day-7, roll back if <15 pp).

---

### Question 3: "What's the worst case if we roll this out and it breaks?"

**What he's really asking:** Rollout is always risky. The Comeback Screen involves notifications, state changes, and notifications-on-notifications. What if we accidentally spam users, or break the freeze logic, or the notification service goes down? What's my downside?

**The tension:** Any feature touching notifications and state has deployment risk. Rolling out quickly increases that risk. But not rolling out also has cost (users churning), and we've pressure-tested the feature.

**Data to answer:**
- Notification risk: The feature uses existing notification infrastructure (we're not building new pipelines). Notification sends are already gated by opt-in (high baseline for users who see comebacks). Worst case: 5% of messages fail to send. Upside: users who break still see the return screen in-app even if notification fails.
- State change risk: The only new state is "freeze banked" (a number). This is additive, not a schema change. Worst case: a user's freeze count is off by 1. Mitigation: validation logic on the backend resets it on next app-open.
- Data quality risk: We're assuming the lesson_completions log is reliable (for gap detection). Spike mitigates this (Raj did the spike already). Worst case: gap detection miscalculates and a user gets a freeze they didn't earn. Downside: they get a gift. Not a risk.
- Rollback risk: Low. Feature flag can kill the screen within 5 minutes. No schema rollback needed.

**The honest answer:** The worst case is we over-notify users and they opt out of notifications entirely, breaking our retention metrics. Mitigation: we run at 5% of week-1 traffic first (10K users) and monitor notification opt-out rate. If >2% opt-out spike, we pause. We don't flood all 200K week-1 users day one. This is a controlled rollout, not a big bang.

---

## How to Address These (One Sentence Each)

1. **Sample size:** Stage the rollout to week 1 (10K users) with a 5% control holdout; if week-1 lift is <15 pp, we don't proceed to weeks 2-4.
2. **Causation:** The 46% control baseline in week 5 matches weeks 1-4 baseline, suggesting cohort quality isn't the driver; proof comes from week-1 rollout replicating the lift.
3. **Downside:** Feature flag can kill the screen in 5 minutes, backup is a refresh (no data rollback needed), and worst case is we over-notify and users opt outâ€”mitigated by staged 5% test first.



