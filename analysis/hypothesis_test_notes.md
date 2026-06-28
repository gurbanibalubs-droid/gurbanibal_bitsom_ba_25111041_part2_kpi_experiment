# Hypothesis Test Notes — Onboarding Campaign Experiment

**Student:** Gurbani Bal | **ID:** BITSOM_BA_25111041
**Dataset:** campaign_experiment_data.xlsx (1400 users after deduplication)

---

## Primary Hypothesis: Paid Conversion Rate

### Business Context

The experiment tests whether the new onboarding and activation campaign causes a meaningful improvement in the share of users who convert to a paid subscription within 30 days. This is the North Star metric for the business because paid conversion directly drives subscription revenue and long-term customer lifetime value.

### Hypothesis Setup

**Null Hypothesis (H0):**
The paid conversion rate of the Treatment group is equal to or less than that of the Control group.
H0: p_treatment <= p_control

**Alternate Hypothesis (H1):**
The paid conversion rate of the Treatment group is greater than that of the Control group.
H1: p_treatment > p_control

**Test Type:** One-tailed (right-tailed) two-proportion z-test

**Significance Level:** alpha = 0.05

**Reason for one-tailed test:** The business decision is directional — we are only interested in whether the Treatment is *better* than Control. A two-tailed test would test for any difference (better or worse), which is not aligned with the decision we need to make.

**Metric Being Tested:** Paid Conversion Rate = Users who converted to paid / Total users in group

**Reason for choosing this metric:** Paid Conversion Rate is the most direct signal of campaign success. It reflects whether the new onboarding experience translates into actual business value (a subscriber paying for the product). All other funnel metrics (landing page visit, trial start, onboarding completion) are intermediate signals; if they improve but paid conversion does not, the campaign has not achieved its goal.

---

### Test Inputs

| Input | Control | Treatment |
|---|---|---|
| Total users (n) | 690 | 710 |
| Users converted to paid (x) | 22 | 50 |
| Observed conversion rate (p) | 3.19% | 7.04% |
| Absolute difference | +3.85 percentage points | |
| Relative lift | +120.7% | |

**Pooled proportion:**
p_pool = (22 + 50) / (690 + 710) = 72 / 1400 = 0.0514

**Standard error:**
SE = sqrt(p_pool * (1 - p_pool) * (1/n_c + 1/n_t))
SE = sqrt(0.0514 * 0.9486 * (1/690 + 1/710))
SE = 0.011807

**Z-statistic:**
Z = (p_treatment - p_control) / SE
Z = (0.0704 - 0.0319) / 0.011807
Z = 3.264

**P-value (one-tailed):** 0.000549

---

### Test Output and Decision Rule

| Output | Value |
|---|---|
| Z-statistic | 3.264 |
| P-value (one-tailed) | 0.000549 |
| Critical value at alpha=0.05 | 1.645 |
| Decision | Reject H0 |

**Decision Rule:** If p-value < 0.05, reject H0 and conclude that the Treatment significantly improves paid conversion rate.

**Result:** p-value = 0.000549 < 0.05. H0 is rejected.

---

### Business Interpretation

The Treatment group achieved a paid conversion rate of 7.04%, compared to 3.19% in the Control group. This 3.85 percentage point improvement is statistically significant at the 5% level (p = 0.00055), meaning there is less than 0.06% probability of observing this large a difference if the Treatment had no true effect.

The result is not a borderline finding — the Z-score of 3.26 is well above the 1.645 threshold, and the p-value is nearly 100 times below the significance threshold. This provides strong statistical evidence that the new onboarding campaign improves paid conversion.

However, statistical significance alone is not sufficient to recommend a full launch. See guardrail analysis below.

---

## Supporting Hypothesis: Engagement Score

### Setup

**H0:** Mean engagement score of Treatment <= mean engagement score of Control
**H1:** Mean engagement score of Treatment > mean engagement score of Control

**Test Type:** One-tailed independent samples t-test
**Significance Level:** alpha = 0.05

### Inputs and Output

| Input | Control | Treatment |
|---|---|---|
| n (non-missing) | 685 | 701 |
| Mean engagement score | 57.03 | 62.94 |
| Difference | +5.91 | |

| Output | Value |
|---|---|
| t-statistic | -7.93 (treatment > control, so magnitude confirms direction) |
| P-value (two-tailed from scipy, halved for one-tailed) | < 0.000001 |
| Decision | Reject H0 |

**Interpretation:** The Treatment group shows a significantly higher average engagement score (62.94 vs 57.03). This confirms that the new onboarding experience not only converts more users but also produces higher engagement among all users, not just those who paid. This is an encouraging finding as it suggests better long-term retention potential among Treatment users.

---

## Guardrail Metric Evaluation

### 1. Refund Rate

| Group | Refund Requests | Refund Rate |
|---|---|---|
| Control | 0 | 0.00% |
| Treatment | 3 | 0.42% |

**Assessment:** The Treatment group has 3 refund requests vs 0 in Control. This is a small absolute number and the rate (0.42%) is well below the typical 2% threshold. A z-test for this comparison produces a p-value of ~0.04 (one-tailed), which is technically significant but the sample of refunds is tiny (n=3) and the practical risk is low.

**Conclusion:** Monitor closely post-launch. Not a blocking concern at this stage, but any increase in refund rate at scale should trigger a review.

---

### 2. Support Ticket Rate

| Group | Avg Support Tickets per User |
|---|---|
| Control | 0.2203 |
| Treatment | 0.3732 |

**Assessment:** Treatment users raised significantly more support tickets (0.37 vs 0.22 per user). This is a 69% increase in support load. This is the most concerning guardrail finding. A higher support ticket rate suggests that some Treatment users may be confused by the new onboarding experience or encountering friction they did not expect after converting. At scale, this could substantially increase operational costs.

**Conclusion:** This is a risk that must be addressed before full launch. The product and support teams should identify the most common ticket categories among Treatment users and resolve the underlying friction points.

---

### 3. Average Revenue per Converted User (ARPCU)

| Group | Converted Users | ARPCU |
|---|---|---|
| Control | 22 | 1,630.10 |
| Treatment | 50 | 770.41 |

**Assessment:** This is the most significant guardrail concern. While the Treatment group converted far more users (50 vs 22), the average revenue per converted user dropped from 1,630 to 770 — a 53% decline. Total revenue (ARPU x all users) is broadly similar between groups (51.97 vs 54.25), meaning the Treatment is converting more users at lower individual spend.

**Interpretation:** The new campaign may be attracting users who are less willing to commit to higher-value plans, or it may be driving conversions through plan types that have lower price points. This needs segment-level investigation by plan type before a full launch decision.

---

### 4. Segment-Level Conversion Decline

Paid conversion rates by region for Control vs Treatment:

| Region | Control Conv. Rate | Treatment Conv. Rate | Direction |
|---|---|---|---|
| North | ~3.0% | ~7.2% | Positive |
| South | ~3.3% | ~7.6% | Positive |
| East | ~3.2% | ~6.4% | Positive |
| West | ~3.4% | ~7.3% | Positive |

No region shows a conversion decline in the Treatment group. The improvement is consistent across all four regions.

**Conclusion:** No segment-level risk from a conversion perspective. However, the ARPCU concern from guardrail 3 should be investigated across plan types specifically.

---

## Summary of Hypothesis and Guardrail Findings

| Test | Result | Decision |
|---|---|---|
| Paid Conversion Rate (Z-test) | p=0.00055, Z=3.26 | Statistically significant improvement |
| Engagement Score (t-test) | p<0.000001, t=-7.93 | Statistically significant improvement |
| Refund Rate | 0.42% vs 0.00% | Low risk, monitor at scale |
| Support Ticket Rate | +69% increase | Medium risk — must investigate |
| ARPCU | -53% drop per converted user | High risk — investigate plan mix |
| Segment consistency | All regions improved | No risk |
