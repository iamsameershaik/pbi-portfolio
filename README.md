# Power BI Portfolio — Sameer Shaik

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?logo=microsoft&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white)

3 end-to-end Power BI dashboards built for a Mid-Level Data Analyst
portfolio. Each project covers the full analytics lifecycle: raw data
→ Power Query cleaning → star schema modelling → DAX measures →
interactive report → business documentation.

## Projects

| # | Project | Domain | Dataset | Status |
|---|---------|--------|---------|--------|
| 1 | [Sales Performance Command Center](./project-01-sales/) | Retail | UCI Online Retail II | ✅ Complete |
| 2 | [Customer Retention & Churn Analytics](./project-02-customer/) | SaaS | IBM Telco Churn | ✅ Complete |
| 3 | [Operations / SLA Dashboard](./project-03-operations/) | Service Ops | NYC 311 | 🔄 In Progress |

## Skills Demonstrated

- **Power Query M** — data cleaning, type handling, derived columns,
  query referencing, advanced editor
- **DAX** — time intelligence, CALCULATE, DIVIDE, VAR/RETURN,
  USERELATIONSHIP, RANKX, PERCENTILEX
- **Data Modelling** — star schema, inactive relationships,
  date tables, cardinality management
- **Report UX** — drill-through, bookmarks, tooltip pages,
  dynamic titles, slicer sync, conditional formatting
- **Documentation** — data dictionaries, KPI glossaries,
  business mini reports

## How to Open Any Project

1. Install Power BI Desktop (free):
   powerbi.microsoft.com/desktop
2. Download the PBIX from the project's /pbix folder
3. Follow the data source instructions in the project's /data/README.md
4. Open in Power BI Desktop — all queries rebuild automatically

## Contact

**LinkedIn:** [linkedin.com/in/iamsameershaik](https://www.linkedin.com/in/iamsameershaik/)

**GitHub:** [github.com/iamsameershaik/pbi-portfolio](https://www.github.com/iamsameershaik/pbi-portfolio)

---

## GitHub Commit Sequence

Do this in order:

git init                          ← if not already a repo
git add .
git commit -m "Initial structure: portfolio scaffold + Project 1 complete

Project 1 - Sales Performance Command Center:
- Star schema: factSales + dimDate + dimProduct + dimCustomer + dimTargets
- 18 DAX measures: Revenue, Time Intelligence, Targets, Ranking, Returns
- 5 report pages + drill-through + tooltip + bookmarks
- Full docs: data dictionary, KPI glossary, mini report"

git remote add origin https://github.com/iamsameershaik/pbi-portfolio.git

git push -u origin main

---

🚧 Portfolio currently under active development.

> Projects 1 and 2 complete. Project 3 in progress.

> Expected completion: 1st April 2026.
