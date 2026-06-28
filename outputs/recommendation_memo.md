# Recommendation Memo — Onboarding Campaign Experiment

**To:** Leadership / Product and Growth Teams
**From:** Gurbani Bal, Business Analyst | BITSOM_BA_25111041
**Re:** Campaign Launch Decision — New Onboarding and Activation Experience
**Date:** June 2025

---

## Executive Summary

The new onboarding and activation campaign produced a statistically significant improvement in paid conversion rate (7.04% Treatment vs 3.19% Control, p = 0.00055) and a meaningful uplift in engagement scores across all users. However, two guardrail metrics — the 69% increase in support ticket rate and the 53% drop in average revenue per converted user — introduce material operational and revenue quality risks that must be addressed before a full launch.

**Recommendation: Launch to a selected segment with guardrail conditions, while investigating support ticket drivers and plan mix before scaling to all users.**

---

## 1. Business Problem Statement

The company is testing a new onboarding and activation campaign to improve user conversion and early engagement. Leadership must decide whether to roll this campaign out to all users.

**Decision required:** Whether to launch the new campaign to all users, a segment of users, or continue testing.

**Who it impacts:** New users at the top of the funnel, the product team, the customer support team, and finance (through revenue mix effects).

**Metric that should improve:** Paid Conversion Rate — the share of users who convert from free/trial to a paid subscription within 30 days.

**Risks to monitor:** Refund rate, support ticket volume, and revenue quality per converted user (ARPCU).

**Evidence required:** Statistical significance on the North Star metric plus no material worsening of guardrail metrics.

---

## 2. North Star Metric

**North Star: Paid Conversion Rate** (converted_to_paid / total users)

**Why this is the North Star:** Paid conversion is the clearest signal that the onboarding experience is successfully moving users from awareness to committed customers. It directly drives subscription revenue. Every other funnel metric — landing page visits, trial starts, onboarding completions — is a means to this end.

**Why other metrics are supporting, not North Star:**
- Landing page visit rate and trial start rate are top-of-funnel signals. They indicate reach and intent but do not confirm revenue impact.
- Engagement score reflects product stickiness but a user can be highly engaged without converting.
- ARPU includes non-converting users and dilutes the revenue signal.

**Risk of optimizing blindly:** If the campaign optimises purely for conversion count without monitoring revenue quality, it may attract users who convert at low price points or churn quickly — improving the metric while degrading long-term business value. This is already visible in the ARPCU data.

---

## 3. KPI Tree Summary

The KPI tree maps the North Star metric (Paid Conversion Rate) to three primary driver pillars:

**Acquisition and Top-of-Funnel:** Landing Page Visit Rate, Trial Start Rate, Traffic Source Mix
**Activation and Onboarding Quality:** Onboarding Completion Rate, Engagement Score, Days to Convert
**Revenue and Retention:** ARPU, ARPCU, Plan Type Upgrade Rate

Four guardrail metrics frame the boundaries: Refund Rate, Support Ticket Rate per User, Segment-Level Conversion Decline, and ARPCU stability.

The KPI tree is available at: `outputs/kpi_tree.png`

---

## 4. Experiment Result Summary

| Metric | Control | Treatment | Lift | Assessment |
|---|---|---|---|---|
| Users | 690 | 710 | — | Balanced groups |
| Landing Page Visit Rate | 63.6% | 72.4% | +8.8pp | Strong improvement |
| Trial Start Rate | 25.1% | 29.0% | +3.9pp | Positive |
| Onboarding Completion Rate | 15.7% | 21.1% | +5.5pp | Strong improvement |
| **Paid Conversion Rate** | **3.19%** | **7.04%** | **+3.85pp** | **Statistically significant** |
| ARPU | 51.97 | 54.25 | +2.28 | Marginal improvement |
| ARPCU | 1,630.10 | 770.41 | -859.69 | Critical concern |
| Refund Rate | 0.00% | 0.42% | +0.42pp | Low risk, monitor |
| Support Ticket Rate | 0.22 | 0.37 | +0.15 | 69% increase, medium risk |
| Engagement Score | 57.03 | 62.94 | +5.91 | Highly significant |
| Days to Convert | 8.86 | 6.40 | -2.46 days | Positive — faster conversion |

---

## 5. Hypothesis Test Interpretation

**Test:** One-tailed two-proportion z-test on paid conversion rate
**H0:** p_treatment <= p_control
**H1:** p_treatment > p_control

**Result:** Z = 3.264, p-value = 0.000549

At alpha = 0.05, we reject H0. The Treatment group's higher conversion rate is statistically significant — there is less than 0.06% probability this difference occurred by chance.

A supporting t-test on engagement score also found a highly significant improvement in the Treatment group (t = -7.93, p < 0.000001).

**Conclusion:** The statistical evidence strongly supports that the campaign drives higher conversion and engagement.

---

## 6. Guardrail Analysis

### Refund Rate
- Control: 0.00% | Treatment: 0.42% (3 refunds from 710 users)
- Risk Level: Low. The absolute number is small. However, the base rate shifting from zero to non-zero is worth monitoring at scale, as 0.42% across tens of thousands of users would generate meaningful refund volume.

### Support Ticket Rate
- Control: 0.22 tickets/user | Treatment: 0.37 tickets/user (+69%)
- Risk Level: Medium. This is the most operationally significant guardrail concern. A 69% increase in support load at scale would strain the support team and raise operating costs. The root cause — whether it is confusion with the new UI, billing questions from newly converted users, or technical issues — must be diagnosed before full launch.

### ARPCU (Revenue Quality)
- Control: 1,630.10 | Treatment: 770.41 (-53%)
- Risk Level: High for revenue quality. The Treatment converts more than twice as many users (50 vs 22), but each converted user generates far less revenue. Total revenue is broadly similar between groups (ARPU of 51.97 vs 54.25), which means the campaign has redistributed conversion rather than grown the revenue pool. This suggests the campaign may be pulling in more Free or Basic plan conversions while reducing Premium conversions. Investigation by plan type is critical.

### Segment Consistency
- All four regions (North, South, East, West) show positive conversion rate improvement in the Treatment group.
- Risk Level: None. No region is being harmed by the campaign.

---

## 7. Segment-Level Insights

**By Region:** Improvement is consistent across all regions. No segment shows a decline in conversion rate. South and West show the strongest absolute lifts.

**By Device:** Mobile users form the largest share of both groups (~61%). The Treatment's onboarding completion and engagement improvement is likely driven partly by mobile experience improvements. Desktop users also show improvement.

**By Traffic Source:** Organic and Paid Search are the largest sources. The Treatment shows Trial Start Rate improvements across all major traffic sources, suggesting the campaign effect is not limited to one acquisition channel.

**Plan Type Concern:** The ARPCU finding points to a likely shift toward Basic or Free plan conversions in the Treatment group. A detailed plan-type breakdown should be conducted before launch to confirm whether Premium conversion is declining.

---

## 8. Final Recommendation: Launch to Selected Segment

**Decision: Do not launch to all users yet. Launch to a controlled segment (Mobile users in all regions, excluding traffic sources with high ARPCU sensitivity) while the following issues are resolved:**

**Conditions before full launch:**
1. Identify and fix the root cause of the 69% support ticket increase. Conduct ticket category analysis and resolve the top 2-3 drivers.
2. Investigate the plan type conversion mix. If the campaign is reducing Premium plan conversions, adjust the messaging or offer structure to protect ARPCU.
3. Monitor refund rate over a 30-day post-launch window for the selected segment. If it exceeds 1.5%, pause and investigate.

**Why not "do not launch" at all:** The conversion and engagement improvements are statistically significant and practically large. Abandoning the campaign entirely based on guardrail concerns that may be addressable would be overcautious.

**Why not full launch immediately:** The ARPCU decline and support ticket increase suggest the campaign has unresolved quality issues. Launching to all users before fixing these could result in higher support costs and lower revenue per user than the Control baseline.

---

## 9. Risks and Limitations

1. **ARPCU concern is the most material risk.** If the campaign is structurally attracting lower-value customers, fixing the support ticket issue will not resolve the revenue quality problem. A full plan-type segmentation analysis is needed.
2. **Short observation window.** Revenue and engagement are measured over 30 days. Long-term retention and churn rates are not captured and may differ between groups.
3. **Missing data.** 18 users have missing device_type and 24 have missing traffic_source. These were excluded from segment-level analyses and may introduce small biases if missingness is not random.
4. **External validity.** The experiment was run on users who signed up during a specific window (early 2025). Seasonal effects or changes in the traffic mix could affect how results generalise to future cohorts.
5. **8 duplicate user_ids** were found and removed (kept first occurrence). The reason for duplication was not determined and may indicate a data pipeline issue.

---

## 10. Next Steps

1. **Immediate (Week 1-2):** Launch to Mobile users across all regions. Set up a support ticket monitoring dashboard with daily alerts.
2. **Short-term (Week 2-4):** Run plan-type segmentation analysis. Interview support team to identify most common ticket categories among Treatment users.
3. **Medium-term (Month 2):** If support ticket rate drops below 0.28/user and ARPCU gap narrows to within 20% of Control, proceed with full rollout.
4. **Ongoing:** Track 90-day retention and churn for both groups to assess long-term campaign impact.
