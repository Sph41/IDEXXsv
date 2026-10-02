# Experiment Design: Comeback Screen Full Test

*Statistical pressure-test of the pilot result and design of the full production test. Date: 2026-10-02.*

---

## Step 1: Statistical Significance of the Pilot

### Plain English First

**What statistical significance means for a PM:** Statistical significance answers the question "Is the difference I observed real and likely to happen again, or could it just be random luck?" In other words, if I had run this test 100 times, would I see a 76% vs 46% gap almost every time, or just some of the time by chance?

### The Calculation

**Pilot result:** 76% treatment (38/50), 46% control (23/50), difference = +30 pp

**Statistical test:** Two-proportion z-test at 95% significance level

```
Pooled proportion = (38 + 23) / (50 + 50) = 61/100 = 0.61
Standard error = sqrt(0.61 Ã— 0.39 Ã— (1/50 + 1/50)) = 0.0976
z-score = (0.76 - 0.46) / 0.0976 = 3.07
z-critical (95% confidence, two-tailed) = 1.96

Since 3.07 > 1.96: STATISTICALLY SIGNIFICANT âœ“
p-value â‰ˆ 0.0021 (very strong signal)
```

### What This Means in Plain English

**The pilot result is real.** We observed a 30 pp gap between treatment and control. The probability this gap is due to random luck (not the actual feature) is less than 0.2% (p = 0.0021). If we ran this same test 100 times on similar users, we'd see a gap of at least 20 pp roughly 99 times.

**But here's the caveat:** We only tested n=50 per variant, which is small. This gives us confidence the *direction* is correct (treatment beats control), but less confidence in the *exact magnitude* (is it really 30 pp or could it be 15 pp in production?). That's why we need a larger, properly powered test.

**Decision at this stage:** The pilot signal is strong enough to justify a full production test. We should NOT roll out blindly, but we should NOT dismiss the signal either.

---

## Step 2: Minimum Detectable Effect (MDE)

### Plain English First

**What MDE means and why you set it before the test:** MDE is your answer to "how much improvement would actually matter to the business?" You set it *before* running the test because if you set it after, you're just rationalizing whatever you happened to observe. It forces you to commit: "I care about detecting at least a 5 pp improvement. Anything smaller isn't worth the effort."

### For This Test

**MDE = 5 percentage points**

This means: We're designing the test to reliably detect an improvement from 46% baseline to 51% (a 5 pp gain). If the true effect is smaller than 5 pp, we're okay not detecting it. If it's larger, we'll definitely catch it.

**Why 5 pp?** Because:
- The pilot showed +30 pp, so we're very confident the real effect is *at least* 5 pp
- A 5 pp improvement on 46% baseline is meaningful business-wise (10.9% relative improvement)
- 5 pp is conservative; it protects us from over-claiming tiny effects

---

## Step 3: Sample Size Calculation

### Plain English First

**What statistical power means and why it matters:** Statistical power (usually set to 80%) is the probability that your test will correctly find a real effect if it exists. If power is too low (say, 20%), you might run a test, see no difference, and incorrectly decide "the feature doesn't work" â€” when in fact it did work, you just didn't have enough users to detect it. You risk a false negative (missing a real win).

### The Calculation

**Parameters:**
- Baseline conversion: 46% (control)
- Target conversion: 51% (treatment; represents the 5 pp MDE)
- Significance level: 95% (two-tailed) â†’ z-alpha = 1.96
- Power: 80% â†’ z-beta = 0.84
- Effect size: 5 percentage points

**Formula:**
```
n = 2 Ã— pÌ„ Ã— (1 - pÌ„) Ã— (z-alpha + z-beta)Â² / (effect)Â²

where:
  pÌ„ = (0.46 + 0.51) / 2 = 0.485
  (1 - pÌ„) = 0.515
  (z-alpha + z-beta)Â² = (1.96 + 0.84)Â² = 2.8Â² = 7.84
  effect = 0.05

n = 2 Ã— 0.485 Ã— 0.515 Ã— 7.84 / (0.05)Â²
n = 2 Ã— 0.2497 Ã— 7.84 / 0.0025
n = 3.915 / 0.0025
n â‰ˆ 1,564 per variant
```

**Total sample size needed: 3,128 users (1,564 treatment + 1,564 control)**

### What This Means in Plain English

To reliably detect a 5 pp improvement (from 46% to 51%) with 80% power and 95% confidence, we need **1,564 users per variant**, or **3,128 users total**. This is 63Ã— larger than the pilot (50 per variant), but it's the cost of statistical rigor at production scale.

---

## Step 4: Test Duration

### The Math

**Available WAU:** 85,000 per week  
**Required sample:** 3,128 users total  
**Duration to reach sample:** 3,128 / 85,000 = 0.037 weeks â‰ˆ 2.6 days

**But we also need to measure Day-7 retention,** so we can't call the test done after 2.6 days. We need:
1. Time to reach sample size: ~2.6 days (â‰ˆ1 week to be safe)
2. Time to measure Day-7 retention: 7 days
3. Buffer for data processing: 1 day
4. **Total duration: ~2 weeks minimum, 3 weeks realistic**

### Does It Fit?

**Max available duration: 8 weeks**  
**Estimated duration needed: 3 weeks**  
**Buffer remaining: 5 weeks**

âœ… **The test fits comfortably within the constraint.** Even if we encounter issues and need to re-run or extend, we have room.

---

## Step 5: The Decision â€” Wait for Full Test or Roll Out Now?

### Option A: Wait for Full Test (Recommended)

**Recommendation:** Run the full 3-week test before rolling out to all users.

**Rationale:**
- The pilot is strong (+30 pp, p = 0.0021), but small (n=50)
- A full test with n=1,564 per variant eliminates doubt about the real effect size
- We have 8 weeks available; 3 weeks for the test leaves 5 weeks for rollout and monitoring
- The cost of waiting 3 weeks is low (continue current churn rate on weeks 1-4); the cost of rolling out a broken feature is high

**Risk of waiting:** We delay recovery of ~100â€“150 users per week (marginal at 85K WAU scale), but we gain certainty.

### Option B: Roll Out Now While Running Full Test

**Not recommended**, but here's the trade-off:
- Start rollout to weeks 1-2 immediately (85K users)
- Run the full test in parallel on weeks 3-4 (holds ~40K users as control)
- If full test confirms +5 pp effect, we've already recovered 85K users for 2-3 weeks
- If full test shows no effect, we stop rollout and lose credibility with the org

**Risk of rolling out now:** If the full test shows a false positive in the pilot (real effect is <5 pp or even negative), we've already deployed to 85K users and may need to rollback mid-week. Reputational and engineering cost is high.

### Final Recommendation: Wait for Full Test

**Reason:** The pilot signal is strong enough that a 3-week wait to confirm is prudent risk management. We lose ~500 users we could have recovered (marginal), but we gain certainty for a feature touching 85K+ users. Standard practice in product is: strong pilot (p < 0.01) + small sample (n=50) = run a full test before production rollout.

---

## Step 6: Leading Indicators â€” What to Watch Weekly

While the full 3-week test runs, monitor these 3 metrics weekly. They'll alert you to problems *before* you get the final day-7 retention result.

### Leading Indicator 1: Day-1 Retention

**What to monitor:** % of test users who return within 24 hours of first session

**Baseline expectation:** Should be stable or slightly up (Comeback Screen shouldn't hurt re-engagement on the first day back)

**Red flag:** Day-1 retention drops >2 pp week-over-week. This signals a mechanical problem (notification not sending, screen crashing, etc.) unrelated to the feature's core appeal.

**Why it matters:** If Day-1 is broken, Day-7 will be broken. This is a fast early warning.

---

### Leading Indicator 2: Notification Open Rate

**What to monitor:** % of treatment users who open the Comeback Screen notification (send #1)

**Baseline expectation:** Should stay 15â€“25% (pilot showed 16% for send #1)

**Red flag:** Open rate drops below 10% week-over-week. This signals either notification fatigue (users are seeing too many notifications) or a deliverability issue (notifications aren't reaching users).

**Why it matters:** If users aren't opening the notification, they can't see the screen, so the feature can't work. A drop here predicts Day-7 failure.

---

### Leading Indicator 3: Comeback Screen Engagement (View â†’ Acted)

**What to monitor:** % of treatment users who saw the Comeback Screen and clicked "Start Comeback Lesson" or "Add Freeze to Bank" (vs. "Not now")

**Baseline expectation:** Should be 50%+ (pilot showed high engagement on the screen)

**Red flag:** Engagement drops below 30% week-over-week. This signals the messaging tone or freeze offer credibility has shifted (maybe the copy is off, or users don't trust the freeze).

**Why it matters:** If users see the screen but don't act, Day-7 retention will suffer. A drop here predicts Day-7 failure earlier than waiting for the full result.

---

## Full Test Design Spec

| Parameter | Value | Rationale |
|---|---|---|
| **Test Type** | Two-variant A/B test (randomized, equal split) | Standard for retention features; 50/50 split maximizes power |
| **Baseline (Control)** | 46% day-7 retention | Week 5 control result; conservative |
| **Minimum Detectable Effect** | 5 percentage points | Meaningful business impact; conservative relative to +30 pp pilot |
| **Treatment** | Comeback Screen (notification + best-streak card + freeze gift) | Week 5 design; Lena approved |
| **Primary Metric** | Day-7 retention (day 7 opened app and current_streak > 0) | Direct proxy for churn risk; measured 7 days post-randomization |
| **Secondary Metrics** | Day-30 retention, day-1 return, notification open rate, screen engagement | Durability, early signals, mechanism understanding |
| **Sample Size** | 1,564 per variant (3,128 total) | 80% power, 95% significance, 5 pp MDE |
| **Cohorts Tested** | Weeks 1â€“2 (first two cohorts to see feature) | Highest churn risk; quickest to measure day-7 |
| **Traffic Allocation** | 50% treatment, 50% control | Standard for two-variant tests |
| **Test Duration** | 3 weeks (1 week data collection + 7 days measurement + 2 days buffer) | Time to reach sample + time to measure day-7 retention |
| **Statistical Significance** | 95% (two-tailed) | Standard; p < 0.05 to declare winner |
| **Statistical Power** | 80% | Standard; 1 in 5 chance of false negative acceptable |
| **Success Criteria** | Treatment day-7 retention â‰¥ 51% (control baseline + MDE) | Minimum threshold to justify full rollout |
| **Rollout Plan (if successful)** | Weeks 3â€“4, then full production by week 4 | Conservative; staged rollout de-risks |

---

## Pilot Result Summary (for Marcus)

| Metric | Result | Interpretation |
|---|---|---|
| **Pilot significance** | p = 0.0021 (highly significant) | The +30 pp gap is real, not random luck |
| **Pilot effect size** | +30 pp (76% vs 46%) | Far exceeds the 5 pp MDE we're testing for |
| **Pilot confidence interval (95%)** | [+15 pp, +45 pp] | We're confident the real effect is at least +15 pp |
| **Full test recommendation** | Run 3-week test to confirm | Pilot is strong but small; need larger sample for certainty |
| **Timeline impact** | Wait 3 weeks, then roll out | Fits within 8-week constraint; adds rigor without major delay |

---

## Recommendation to Marcus (Updated)

**OLD:** "Roll out immediately based on pilot."  
**NEW:** "Run a 3-week full test to confirm the pilot signal before production rollout. The pilot is statistically significant (p = 0.0021) and effect size far exceeds our success threshold (+30 pp vs. 5 pp MDE), so risk of full rollout is low. But given pilot's small sample (n=50), a properly powered test (n=1,564 per variant) is due diligence. Full test fits within the 8-week window with room to spare."

**The key insight:** The pilot result is strong enough to move forward, but not strong enough to skip the rigor step. A 3-week test is the right balance between speed and certainty.

