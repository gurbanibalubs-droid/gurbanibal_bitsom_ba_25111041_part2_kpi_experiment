# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

**Student:** Gurbani Bal
**Student ID:** BITSOM_BA_25111041
**Program:** Business Analytics — BITSOM
**Repository:** `gurbanibal_bitsom_ba_25111041_part2_kpi_experiment`

---

## Business Context

A subscription-based digital product company launched a new onboarding and activation campaign. Users were split into two groups:

- **Control:** Existing onboarding experience
- **Treatment:** New campaign experience

Leadership wants to know whether the Treatment campaign should be rolled out to all users. This analysis frames the business problem, defines success metrics, analyzes the experiment data, evaluates guardrail metrics, and delivers a data-backed recommendation.

---

## Dataset Description

| Attribute | Detail |
|---|---|
| File | `campaign_experiment_data.xlsx` |
| Sheet | `experiment_data` |
| Raw records | 1,408 rows |
| Records after deduplication | 1,400 rows (8 duplicate user_ids removed) |
| Columns | 16 (user_id, signup_date, experiment_group, region, device_type, traffic_source, plan_type, visited_landing_page, started_trial, completed_onboarding, converted_to_paid, revenue_30d, support_tickets_30d, refund_requested, days_to_convert, engagement_score) |
| Groups | Control: 690 users, Treatment: 710 users |
| Observation window | 30 days post signup |

---

## North Star Metric

**Paid Conversion Rate** — the share of users who convert from free/trial to a paid subscription within 30 days.

This was selected because it most directly reflects the business goal of the campaign (growing paying subscribers) and is the clearest signal that the onboarding experience is translating into revenue. All funnel metrics upstream (visits, trials, onboarding completions) are drivers of this metric, not replacements for it.

**Risk of blind optimisation:** The experiment revealed that while Treatment converted more users, average revenue per converted user dropped by 53%, suggesting conversion count alone is insufficient as a success criterion.

---

## KPI Tree Summary

The North Star (Paid Conversion Rate) is driven by three pillars:

**1. Acquisition and Top-of-Funnel**
- Landing Page Visit Rate
- Trial Start Rate
- Traffic Source Mix (Quality)

**2. Activation and Onboarding Quality**
- Onboarding Completion Rate
- Engagement Score (Avg)
- Days to Convert

**3. Revenue and Retention**
- Average Revenue per User (ARPU)
- Average Revenue per Converted User (ARPCU)
- Plan Type Upgrade Rate

**Guardrail Metrics:**
- Refund Rate (threshold: <= 2%)
- Support Ticket Rate per User
- Segment-Level Conversion Decline
- ARPCU Stability

KPI tree image: `outputs/kpi_tree.png`

---

## Experiment Analysis Approach

### Data Quality Checks Performed
| Check | Finding | Action |
|---|---|---|
| Duplicate user_ids | 8 duplicates | Removed (kept first occurrence) |
| Missing device_type | 18 records | Retained; excluded from device analysis |
| Missing traffic_source | 24 records | Retained; excluded from source analysis |
| Missing engagement_score | 14 records | Mean calculated over available values |
| Missing days_to_convert | 1,336 records | Expected — only converted users have values |
| Invalid binary values | None | All binary fields contain only 0 or 1 |
| Revenue outliers (>2000) | Present | Retained — valid high-value conversions |

### Group Balance
Control and Treatment groups are well balanced across regions, device types, traffic sources, and plan types. No significant imbalance was found that would confound the analysis.

---

## Hypothesis Test Summary

**Test:** One-tailed two-proportion z-test on Paid Conversion Rate
**H0:** p_treatment <= p_control
**H1:** p_treatment > p_control
**Significance level:** alpha = 0.05

| Input / Output | Value |
|---|---|
| Control conversion rate | 3.19% (22/690) |
| Treatment conversion rate | 7.04% (50/710) |
| Absolute lift | +3.85 percentage points |
| Pooled proportion | 5.14% |
| Z-statistic | 3.264 |
| P-value (one-tailed) | 0.000549 |
| Decision | Reject H0 |

**Result:** The improvement is statistically significant. There is less than 0.06% probability this difference occurred by chance.

A supporting t-test on engagement score also confirmed a highly significant improvement in the Treatment group (t = -7.93, p < 0.000001).

Full test workings and interpretation: `analysis/hypothesis_test_notes.md`

---

## Guardrail Metrics Considered

| Guardrail | Control | Treatment | Risk Level |
|---|---|---|---|
| Refund Rate | 0.00% | 0.42% | Low — monitor at scale |
| Support Ticket Rate | 0.22/user | 0.37/user (+69%) | Medium — investigate root cause |
| ARPCU | 1,630 | 770 (-53%) | High — investigate plan mix |
| Segment Conversion Decline | — | All regions positive | None |

The ARPCU drop and support ticket increase are the two material risks that prevent an immediate full launch recommendation.

---

## Final Recommendation

**Launch to a selected segment (Mobile users across all regions), with conditions:**

1. Identify and fix the root cause of the 69% support ticket rate increase before full rollout.
2. Conduct a plan-type segmentation analysis to understand whether Premium plan conversions are declining in the Treatment group and adjust campaign messaging if so.
3. Monitor refund rate over 30 days post-launch. If it exceeds 1.5%, pause and investigate.

Full reasoning: `outputs/recommendation_memo.md`

---

## Assumptions and Limitations

1. **30-day observation window only.** Long-term retention, churn, and lifetime value are not captured and may differ between groups.
2. **Days to convert is only available for converted users.** The 1,336 missing values are structurally expected and do not indicate a data quality issue.
3. **ARPCU comparison has small sample sizes** — 22 Control converters vs 50 Treatment converters. The -53% gap is large enough to be directionally reliable, but should be re-evaluated at scale.
4. **Missing device_type and traffic_source** for a small number of users (18 and 24 respectively) were excluded from segment-level analyses.
5. **Root cause of 8 duplicate user_ids** was not determined. This may indicate a pipeline issue and should be investigated with the data engineering team.
6. **Plan type analysis was not included in the provided dataset segment breakdowns.** The ARPCU finding is inferred from group-level revenue data and requires a dedicated plan-type breakdown for full validation.

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment main metrics summary table |
| `screenshots/hypothesis_test_output.png` | Z-test calculation and p-value output |
| `screenshots/kpi_tree_preview.png` | KPI tree image preview |

---

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx
├── analysis/
│   ├── experiment_analysis.xlsx
│   └── hypothesis_test_notes.md
├── outputs/
│   ├── kpi_tree.png
│   ├── experiment_summary.xlsx
│   └── recommendation_memo.md
├── screenshots/
│   ├── summary_metrics.png
│   ├── hypothesis_test_output.png
│   └── kpi_tree_preview.png
└── README.md
```
