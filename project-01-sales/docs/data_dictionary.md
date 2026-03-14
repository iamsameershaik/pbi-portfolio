# Data Dictionary — Project 01: Sales Performance Command Center

## Dataset
- Name: UCI Online Retail II
- Source: UC Irvine Machine Learning Repository
- URL: https://archive.ics.uci.edu/dataset/502/online+retail+ii
- Licence: CC BY 4.0
- Raw rows: ~1,067,371
- Valid sales rows: ~824,000
- Date range: December 2009 — December 2011

---

## Table: factSales
Grain: One row per line item per invoice.
A single invoice (order) contains multiple line items (products).

| Column | Data Type | Source | Business Meaning | Transform Notes |
|---|---|---|---|---|
| Invoice | Text | Raw | Unique invoice number. Prefix 'C' indicates a credit note (return). | Retained as-is. Used in DISTINCTCOUNT for order counts. |
| StockCode | Text | Raw | Product SKU / stock keeping unit. Foreign key to dimProduct. | Uppercased and trimmed via Text.Upper(Text.Trim()) to resolve case inconsistencies in source data. |
| Quantity | Integer | Raw | Units sold per line item. Negative values indicate returns. | Retained as-is. Used in c_Revenue calculation. |
| Price | Decimal | Raw | Unit price in GBP at time of transaction. | Retained as-is. Rows with Price <= 0 excluded via c_IsValidSale flag. |
| Customer ID | Text | Raw | Unique customer identifier. Nullable — ~25% of rows have no CustomerID (guest orders). | Retained including nulls. Flagged via c_IsGuestOrder. |
| Country | Text | Raw | Customer country of origin. | Retained as-is. Used in dimCustomer. |
| DateKey | Integer | Derived | YYYYMMDD integer key for joining to dimDate. e.g. 20110115. | Calculated in Power Query: Year*10000 + Month*100 + Day. |
| c_Revenue | Decimal | Derived | Revenue for this line item: Quantity × Price. | Calculated in Power Query. Data type set to Decimal Number explicitly. |
| c_IsReturn | Boolean | Derived | TRUE if Invoice starts with 'C' (credit note). | Calculated in Power Query: Text.StartsWith([Invoice], "C"). |
| c_IsValidSale | Boolean | Derived | TRUE if Quantity > 0 AND Price > 0 AND not a return. Used as primary sales filter. | Calculated in Power Query: [Quantity] > 0 and [Price] > 0 and not [c_IsReturn]. |
| c_IsGuestOrder | Boolean | Derived | TRUE if Customer ID is null (guest checkout). | Calculated in Power Query: [Customer ID] = null. |

---

## Table: dimProduct
Grain: One row per unique StockCode.
Built from factSales_raw via Group By + deduplication.

| Column | Data Type | Source | Business Meaning | Transform Notes |
|---|---|---|---|---|
| StockCode | Text | Raw | Primary key. Unique product identifier. | Uppercased and trimmed. Deduplicated via Table.Distinct after sort. |
| Description | Text | Raw | Product name / description. | First alphabetical description kept per StockCode after sort. |
| Price | Decimal | Raw | Reference unit price. Note: this is the first recorded price, not a max or average. Does not affect revenue calculations. | Retained from first row after sort and deduplication. |
| Category | Text | Derived | Rule-based product category derived from Description keywords. | Calculated in Power Query using Text.Contains logic. Categories: General, Bags, Home Decor, Lighting, Kitchen, Seasonal, Vintage, Garden, Storage. |

---

## Table: dimCustomer
Grain: One row per unique Customer ID.
Built from factSales_raw via deduplication. Excludes null Customer IDs.

| Column | Data Type | Source | Business Meaning | Transform Notes |
|---|---|---|---|---|
| Customer ID | Text | Raw | Primary key. Unique customer identifier. | Null rows removed — nulls represent guest orders and are not valid dimension members. |
| Country | Text | Raw | Customer's country. Represents the most recent country on record for this customer. | Retained as-is after deduplication. |
| c_Segment | Text | Derived | Customer value segment. Currently set to 'Standard' as placeholder. | To be replaced with DAX RFM segmentation in future iteration. |

---

## Table: dimDate
Grain: One row per calendar date.
Built as a DAX calculated table. Marked as official date table.
Range: 1 January 2009 — 31 December 2012 (1,461 rows).

| Column | Data Type | Business Meaning | Notes |
|---|---|---|---|
| Date | Date | Calendar date. Primary key and official date table column. | Used in all time intelligence functions. |
| DateKey | Integer | YYYYMMDD integer. Foreign key join to factSales[DateKey]. | e.g. 20110115 |
| Year | Integer | Calendar year. | e.g. 2011 |
| QuarterNo | Integer | Quarter number 1–4. | e.g. 3 |
| Quarter | Text | Quarter label. | e.g. Q3 |
| MonthNo | Integer | Month number 1–12. Used as sort column for MonthName and MonthYear. | e.g. 9 |
| MonthName | Text | Abbreviated month name. Sorted by MonthNo. | e.g. Sep |
| MonthYear | Text | Month and year label. Sorted by MonthYearSort. | e.g. Sep 2011 |
| MonthYearSort | Integer | Integer sort key for MonthYear: Year × 100 + MonthNo. | e.g. 201109 |
| WeekNo | Integer | ISO week number. | e.g. 37 |
| DayOfWeekNo | Integer | Day of week number (1=Monday, 7=Sunday). | e.g. 4 |
| DayName | Text | Abbreviated day name. | e.g. Thu |
| IsWeekend | Boolean | TRUE if Saturday or Sunday. | Used for weekday/weekend analysis. |

---

## Table: dimTargets
Grain: One row per calendar month.
Built via Enter Data in Power BI Desktop.
Range: January 2010 — December 2011 (24 rows).
Note: Illustrative targets created for portfolio demonstration only.

| Column | Data Type | Business Meaning | Notes |
|---|---|---|---|
| YearMonth | Text | Month identifier in YYYY-MM format. Foreign key to dimDate[YearMonth]. | e.g. 2011-09 |
| TargetRevenue | Integer | Monthly revenue target in GBP. | Illustrative values only — not from source data. |

---

## Relationships

| From Table | From Column | To Table | To Column | Cardinality | Direction | Active |
|---|---|---|---|---|---|---|
| dimDate | DateKey | factSales | DateKey | 1:* | Single | Yes |
| dimProduct | StockCode | factSales | StockCode | 1:* | Single | Yes |
| dimCustomer | Customer ID | factSales | Customer ID | 1:* | Single | Yes |
| dimTargets | YearMonth | dimDate | YearMonth | 1:* | Single | No (inactive) |

---

## Known Data Quality Issues

| Issue | Impact | Resolution |
|---|---|---|
| ~25% of transactions have null CustomerID | Customer-level metrics exclude these rows | Flagged via c_IsGuestOrder; included in revenue totals |
| StockCode case inconsistency (e.g. 84993a vs 84993A) | Caused duplicate SKUs in dimProduct | Resolved via Text.Upper(Text.Trim()) in Power Query |
| Some StockCodes are non-product codes (POST, DOT, M etc.) | Would inflate product count and distort analysis | Filtered out via c_IsNonProduct flag in dimProduct build |
| TotalCharges column not present | No lifetime value calculation possible | Revenue calculated from Quantity × Price per transaction |
| No cost or margin data | Profitability analysis not possible | Revenue analysis only — noted in report limitations |