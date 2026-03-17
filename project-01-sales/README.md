# Project 1 — Sales Performance Command Center

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?logo=microsoft&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white)

## Overview
A weekly sales analytics dashboard built for a Sales Director persona.
Tracks revenue, growth, product mix, and regional performance across
~824,000 valid transactions from a UK-based online gift retailer.

## Business Questions Answered
1. What is revenue trend WoW / MoM / YoY?
2. Which categories are driving growth or decline?
3. Which countries generate the most revenue?
4. What is the return rate by product?
5. How does actual revenue compare to targets?

## Dataset
- Source: UCI Online Retail II
- URL: archive.ics.uci.edu/dataset/502/online+retail+ii
- Licence: CC BY 4.0
- Rows: ~1,067,371 raw | ~824,000 valid sales
- Date range: Dec 2009 — Dec 2011

## Data Model
Star schema: factSales (transaction grain) + 4 dimensions
- dimDate — marked as date table, 2009–2012
- dimProduct — SKU level with rule-based categories
- dimCustomer — customer + country
- dimTargets — manual monthly targets 2010–2011

## Key DAX Measures
| Measure | Purpose |
|---|---|
| m_Total Revenue | Valid sales revenue |
| m_Revenue YoY % | Year-on-year growth |
| m_Rolling 12M Revenue | Trailing 12-month trend |
| m_Revenue vs Target % | Actual vs target variance |
| m_Return Rate | Returns as % of all orders |

## Report Pages
1. Executive Overview — KPIs, trend, map
2. Product & Category — category mix, top products, returns
3. Region Performance — country matrix, filled map
4. Drivers — decomposition tree, variance matrix
5. Definitions & About — KPI glossary, data source, limitations

## How to Open
1. Install Power BI Desktop (free): powerbi.microsoft.com/desktop
2. Download SalesCommandCenter.pbix from /pbix
3. Open in Power BI Desktop — data is embedded (import mode)
4. Use Year slicer to filter; right-click country bars to drill through

## Screenshots
See /screenshots folder for full report evidence.

## Built By
Sameer Shaik — Mid-Level Data Analyst Portfolio

**LinkedIn:** [linkedin.com/in/iamsameershaik](https://www.linkedin.com/in/iamsameershaik/)

**GitHub:** [github.com/iamsameershaik/pbi-portfolio](https://www.github.com/iamsameershaik/pbi-portfolio)

---

## Step 6 — GitHub Commit

In your terminal or GitHub Desktop:

git add .
git commit -m "Project 01: Sales Performance Command Center — complete

- Star schema: factSales + dimDate + dimProduct + dimCustomer + dimTargets
- 18 DAX measures across 5 groups
- 5 report pages + drill-through + tooltip + bookmarks
- Full documentation: data dictionary, KPI glossary, mini report"

git push origin main
