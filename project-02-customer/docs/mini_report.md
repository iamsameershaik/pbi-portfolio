# Customer Retention & Churn Analysis — IBM Telco Churn Dataset

## Background
Analysis of 7,043 telecom customers to identify churn drivers, retention
patterns, and revenue at risk. The dataset covers a cross-sectional
snapshot of customers across three contract types, four payment methods,
and multiple service configurations. This report was built using Power BI
Desktop with a star schema model and 15 DAX measures targeting a
Head of Growth persona.

## Key Findings

1. **Overall churn rate is 26.5%, representing 1,869 lost customers
   and $1.67M in annualised revenue at risk ($139.13K MRR).** This is
   a critical business problem — more than 1 in 4 customers is leaving,
   and the financial exposure is substantial relative to the customer
   base size.

2. **Contract type is the single strongest churn predictor.**
   Month-to-month customers churn at 42.7% vs 11.3% for one-year
   and just 2.8% for two-year contracts — a 15× difference between
   the extremes. Customers who commit to longer contracts are
   dramatically more likely to stay, making contract conversion
   the highest-leverage retention lever available.

3. **The early lifecycle is the highest-risk window.** Churned
   customers have an average tenure of just 18.0 months vs 37.6
   months for active customers — a 20-month gap. The cohort matrix
   shows month-to-month customers in the 0-12M band churning at
   51.4% — more than half are leaving before their first year is
   complete. Early engagement and onboarding programmes are critical.

4. **Fiber optic internet customers churn at 41.9% — more than
   double the DSL rate of 19.0% and nearly 6× the no-internet rate
   of 7.4%.** This is a significant service quality or value
   perception issue. Drill-through analysis confirms the highest
   risk segment is fiber optic + month-to-month customers, many
   of whom churn within the first 1-3 months of service.

5. **Electronic check payment method correlates with the highest
   churn rate at 45.3%** — nearly 3× the credit card rate of 15.2%.
   This may indicate a lower-commitment customer profile associated
   with manual payment methods, or friction in the payment experience
   itself. A relatively simple intervention — encouraging auto-pay
   adoption — could meaningfully reduce churn in this segment.

## Recommendations

1. **Launch a contract conversion programme targeting month-to-month
   customers in their first 12 months.** The 51.4% churn rate in
   this cohort represents the single largest retention opportunity.
   Incentivising annual contract sign-ups (e.g. first month free,
   locked pricing) could move customers into the 11.3% churn bracket
   and recover a significant share of the $1.67M annual revenue at risk.
   Even a 5% reduction in churn saves $72.62K annually.

2. **Investigate fiber optic service quality urgently.** A 41.9%
   churn rate strongly suggests customers are not receiving the value
   they expected. This warrants an NPS/CSAT deep-dive specifically
   for fiber optic customers in their first 90 days — the drill-through
   analysis shows churn occurring as early as month 1 in this segment,
   pointing to an onboarding or expectation-setting failure.

3. **Implement an auto-pay migration campaign targeting electronic
   check users.** With a 45.3% churn rate vs 15.2% for credit card
   users, shifting customers to automatic payment methods reduces
   friction, improves payment reliability, and correlates strongly
   with retention. This is a low-cost, high-impact intervention
   that requires no product changes.

## If This Were Production

- Enrich with support ticket and usage data to build leading
  indicators of churn — customers who reduce usage or raise
  multiple support tickets in month 1-3 are likely pre-churn
  signals that could trigger automated retention outreach.
- Build a churn propensity score using logistic regression or
  a simple decision tree in Python/R, then surface the top 100
  at-risk customers weekly in a Power BI report for the
  retention team to action directly.

## If I Had More Data

- **Support ticket history** — correlate complaint volume and
  resolution time with churn probability
- **Usage data** — identify low-engagement customers as early
  churn signals before they formally cancel
- **Actual subscription dates** — build true time-series cohort
  retention curves rather than tenure band proxies
- **Marketing campaign data** — measure whether acquisition
  channel predicts churn rate (e.g. discount-acquired customers
  may churn faster)

## Data Limitations

- Dataset is a point-in-time snapshot — no true time series
  available, cohort analysis uses tenure bands as a proxy.
- CLV figures are proxy calculations (tenure × monthly charges)
  with no discount rate or renewal probability model applied —
  treat as directional indicators only.
- No usage or behavioural data available — churn drivers are
  limited to demographic and service configuration variables.
- Targets table not applicable for this project — no revenue
  targets exist in the source data.