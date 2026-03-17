# Project 2 — Customer Retention & Churn Analytics

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?logo=microsoft&logoColor=white)
![CSV](https://img.shields.io/badge/CSV-217346?logo=microsoft-excel&logoColor=white)

## Overview
A customer retention analytics dashboard built for a Head of Growth
persona. Analyses churn drivers, tenure cohorts, customer lifetime
value, and segment characteristics across 7,043 telecom customers.
Identifies $1.67M in annualised revenue at risk and quantifies the
impact of targeted retention interventions.

## Business Questions Answered
1. What is the overall churn rate and how much revenue is at risk?
2. Which contract type has the highest churn rate?
3. Where in the customer lifecycle does churn peak?
4. Which internet service type correlates with highest churn?
5. What is the financial impact of reducing churn by 5%?
6. How does proxy CLV differ between active and churned customers?
7. Which payment method correlates with highest churn?
8. What does the churn rate matrix look like by tenure and contract?
9. How does avg tenure differ between churned and active customers?
10. Which customer segment represents the highest combined risk?

## Dataset
- Source: IBM Telco Customer Churn via Kaggle
- URL: kaggle.com/datasets/blastchar/telco-customer-churn
- Licence: CC0 — Public Domain
- Rows: 7,043 customers | Columns: 21
- Note: Point-in-time snapshot — no date series available

## Data Model
Star schema: factSubscription (customer snapshot grain) +
3 dimension tables

| Table | Type | Rows | Purpose |
|---|---|---|---|
| factSubscription | Fact | 7,043 | One row per customer — all metrics |
| dimCustomer | Dimension | 7,043 | Customer demographics and service config |
| dimTenureBand | Dimension | 6 | Tenure groupings with sort order |
| dimContract | Dimension | 3 | Contract type classifications |

## Key DAX Measures

| Measure | Purpose |
|---|---|
| m_Churn Rate | 26.5% overall — snapshot churn rate |
| m_MRR at Risk | $139.13K monthly revenue lost to churn |
| m_Annual Revenue at Risk | $1.67M annualised revenue at risk |
| m_Proxy CLV | Lifetime value proxy (tenure × monthly charges) |
| m_5pct Churn Reduction Impact | $72.62K annual saving from 5% churn reduction |
| m_Cohort Churn Rate | Churn rate within tenure band context |

## Key Findings
- Month-to-month customers churn at 42.7% vs 2.8% for two-year contracts
- Highest risk cohort: month-to-month + 0-12M tenure = 51.4% churn
- Fiber optic churn rate: 41.9% vs DSL 19.0%
- Electronic check payment method: 45.3% churn vs credit card 15.2%
- Churned customers leave after avg 18.0 months vs 37.6 for active

## Report Pages
1. Retention Overview — KPIs, churn by contract, tenure trend
2. Cohort & Tenure — matrix, distribution, scatter plot
3. Segment Drivers — internet service, payment method, senior status
4. Customer Detail — drill-through from any segment
5. Definitions & About — KPI glossary, data source, limitations

## How to Open
1. Install Power BI Desktop (free): powerbi.microsoft.com/desktop
2. Download CustomerChurnAnalytics.pbix from /pbix
3. Follow data source instructions in /data/README.md
4. Open in Power BI Desktop — data is embedded (import mode)
5. Use Contract slicer to filter; right-click contract bars
   to drill through to customer detail

## Screenshots
See /screenshots folder for full report evidence.

## Built By
Sameer Shaik — Mid-Level Data Analyst Portfolio

**LinkedIn:** [linkedin.com/in/iamsameershaik](https://www.linkedin.com/in/iamsameershaik/)

**GitHub:** [github.com/iamsameershaik/pbi-portfolio](https://www.github.com/iamsameershaik/pbi-portfolio)
