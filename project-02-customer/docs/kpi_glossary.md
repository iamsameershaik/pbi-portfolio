# KPI Glossary — Project 02: Customer Retention & Churn Analytics

## Overview
This glossary defines all 15 DAX measures built in the _Measures table.
All measures are prefixed with m_ and stored in display folders.

---

## Display Folder: CUSTOMER COUNTS

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Total Customers | Count of all customers regardless of churn status | COUNTROWS on factSubscription | #,0 | ↑ Higher is better |
| m_Churned Customers | Count of customers where Churn = Yes | CALCULATE with c_IsChurned = 1 | #,0 | ↓ Lower is better |
| m_Active Customers | Count of customers where Churn = No | CALCULATE with c_IsChurned = 0 | #,0 | ↑ Higher is better |

---

## Display Folder: CHURN METRICS

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Churn Rate | Churned customers as a percentage of total customers | DIVIDE(m_Churned Customers, m_Total Customers) | 0.0% | ↓ Lower is better |
| m_MRR at Risk | Sum of monthly charges for churned customers only | SUMX filtered by c_IsChurned = 1 | $#,0.00 | ↓ Lower is better |
| m_Annual Revenue at Risk | MRR at Risk annualised | m_MRR at Risk × 12 | $#,0.00 | ↓ Lower is better |
| m_Avg Tenure Churned | Average tenure in months for churned customers only | CALCULATE(AVERAGE(tenure), c_IsChurned = 1) | 0.0 | Reference only |
| m_Avg Tenure Active | Average tenure in months for active customers only | CALCULATE(AVERAGE(tenure), c_IsChurned = 0) | 0.0 | Reference only |

---

## Display Folder: REVENUE

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Total MRR | Sum of monthly charges across all customers | SUM(MonthlyCharges) | $#,0.00 | ↑ Higher is better |
| m_ARPU | Average monthly revenue per customer | DIVIDE(m_Total MRR, m_Total Customers) | $#,0.00 | ↑ Higher is better |
| m_Proxy CLV | Average lifetime value proxy across all customers | AVERAGEX(tenure × MonthlyCharges) | $#,0.00 | ↑ Higher is better |
| m_Proxy CLV Churned | Proxy CLV for churned customers only | CALCULATE(m_Proxy CLV, c_IsChurned = 1) | $#,0.00 | Reference only |
| m_Proxy CLV Active | Proxy CLV for active customers only | CALCULATE(m_Proxy CLV, c_IsChurned = 0) | $#,0.00 | Reference only |

---

## Display Folder: SCENARIO

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_5pct Churn Reduction Impact | Estimated annual revenue saved by reducing churn by 5% | (Churned × 0.05) × ARPU × 12 | $#,0.00 | ↑ Higher is better |

---

## Display Folder: COHORT

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Cohort Churn Rate | Churn rate calculated within tenure band context only | CALCULATE(m_Churn Rate, ALLEXCEPT(factSubscription, c_TenureBand)) | 0.0% | ↓ Lower is better |

---

## Denominator Notes

| Measure | Denominator Clarification |
|---|---|
| m_Churn Rate | Denominator = all customers (snapshot rate, not period rate) |
| m_MRR at Risk | Includes only churned customers — represents revenue already lost |
| m_Annual Revenue at Risk | Assumes no replacement customers — directional estimate only |
| m_ARPU | Denominator includes both active and churned customers |
| m_Proxy CLV | Proxy only — no discount rate or renewal probability applied |
| m_5pct Churn Reduction Impact | Based on current ARPU — assumes saved customers pay average rate |
| m_Cohort Churn Rate | Removes all filters except TenureBand — use in matrix visuals only |

---

## Red / Amber / Green Thresholds

| KPI | Green | Amber | Red |
|---|---|---|---|
| m_Churn Rate | < 15% | 15–25% | > 25% |
| m_Annual Revenue at Risk | < $500K | $500K–$1M | > $1M |
| m_Avg Tenure Churned | > 24M | 12–24M | < 12M |
| m_ARPU | > $70 | $50–$70 | < $50 |