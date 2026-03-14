# KPI Glossary — Project 01: Sales Performance Command Center

## Overview
This glossary defines all 18 DAX measures built in the _Measures table.
All measures are prefixed with m_ and stored in display folders.

---

## Display Folder: REVENUE

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Total Revenue | Sum of all valid sales revenue (Quantity × Price). Excludes returns and zero-price rows. | SUMX on c_Revenue filtered by c_IsValidSale = TRUE | £#,0.00 | ↑ Higher is better |
| m_Total Orders | Count of distinct invoice numbers for valid sales only. One invoice = one order. | DISTINCTCOUNT on Invoice filtered by c_IsValidSale = TRUE | #,0 | ↑ Higher is better |
| m_Average Order Value | Total revenue divided by total orders. Measures spend per transaction. | DIVIDE(m_Total Revenue, m_Total Orders) | £#,0.00 | ↑ Higher is better |
| m_Total Units Sold | Sum of all units sold for valid sales. Excludes return quantities. | SUM on Quantity filtered by c_IsValidSale = TRUE | #,0 | ↑ Higher is better |

---

## Display Folder: TIME INTELLIGENCE

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Revenue SPLY | Total revenue for the same period in the prior year. Used as denominator in YoY calculation. | CALCULATE with SAMEPERIODLASTYEAR | £#,0.00 | Reference only |
| m_Revenue YoY % | Year-on-year revenue growth as a percentage. Positive = growth, negative = decline. | DIVIDE(current - prior, prior) using m_Revenue SPLY | 0.0% | ↑ Higher is better |
| m_Revenue SPLM | Total revenue for the same period in the prior month. Used as denominator in MoM calculation. | CALCULATE with DATEADD -1 MONTH | £#,0.00 | Reference only |
| m_Revenue MoM % | Month-on-month revenue growth as a percentage. | DIVIDE(current - prior, prior) using m_Revenue SPLM | 0.0% | ↑ Higher is better |
| m_Rolling 12M Revenue | Sum of revenue over the trailing 12 months from the last date in context. Smooths seasonality. | CALCULATE with DATESINPERIOD -12 MONTH | £#,0.00 | ↑ Higher is better |
| m_Revenue WoW % | Week-on-week revenue growth as a percentage. | DIVIDE(current - prior, prior) using DATEADD -7 DAY | 0.0% | ↑ Higher is better |

---

## Display Folder: TARGETS

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Target Revenue | Monthly revenue target from dimTargets table. Covers 2010–2011 only. Note: illustrative targets for portfolio demonstration. | SUM on TargetRevenue using USERELATIONSHIP with dimDate[YearMonth] | £#,0.00 | Reference only |
| m_Revenue vs Target | Absolute difference between actual revenue and target. Positive = above target, negative = below. | m_Total Revenue minus m_Target Revenue | £#,0.00 | ↑ Higher is better |
| m_Revenue vs Target % | Actual revenue as a percentage above or below target. | DIVIDE(m_Revenue vs Target, m_Target Revenue) | 0.0% | ↑ Higher is better |

---

## Display Folder: RANKING

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Category Revenue Share % | A category's revenue as a percentage of total revenue. Denominator removes all category filters. | DIVIDE(m_Total Revenue, CALCULATE with ALL on Category) | 0.0% | Contextual |
| m_Product Rank by Revenue | Ranks products by revenue within the current filter context. Rank 1 = highest revenue. | RANKX with ALLSELECTED on Description | 0 | ↓ Lower rank = better |
| m_Top 10 Revenue Share | Revenue of the top 10 products as a percentage of total revenue. Measures SKU concentration. | TOPN(10) + SUMX divided by total revenue with ALL | 0.0% | Contextual |

---

## Display Folder: RETURNS

| Measure | Business Definition | DAX Logic | Format | Direction |
|---|---|---|---|---|
| m_Return Orders | Count of distinct credit note invoices (invoices starting with 'C'). | DISTINCTCOUNT on Invoice filtered by c_IsReturn = TRUE | #,0 | ↓ Lower is better |
| m_Return Rate | Return orders as a percentage of all orders (valid + returns). | DIVIDE(m_Return Orders, m_Total Orders + m_Return Orders) | 0.0% | ↓ Lower is better |

---

## Denominator Notes

| Measure | Denominator Clarification |
|---|---|
| m_Revenue YoY % | Returns BLANK when no prior year data exists in filter context |
| m_Revenue MoM % | Returns BLANK when no prior month data exists in filter context |
| m_Revenue vs Target % | Returns BLANK outside the 2010–2011 date range |
| m_Category Revenue Share % | Denominator is total revenue with ALL filters removed from Category |
| m_Return Rate | Denominator includes both valid orders AND return orders |
| m_Product Rank by Revenue | Rank resets when filter context changes (e.g. year slicer) |

---

## Red / Amber / Green Thresholds (Suggested)

| KPI | Green | Amber | Red |
|---|---|---|---|
| m_Revenue YoY % | > 5% | 0% to 5% | < 0% |
| m_Revenue vs Target % | > 0% | -10% to 0% | < -10% |
| m_Return Rate | < 2% | 2% to 5% | > 5% |
| m_Average Order Value | > £600 | £400–£600 | < £400 |