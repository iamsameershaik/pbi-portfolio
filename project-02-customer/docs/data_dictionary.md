# Data Dictionary — Project 02: Customer Retention & Churn Analytics

## Dataset
- Name: IBM Telco Customer Churn
- Source: Kaggle (blastchar)
- URL: https://kaggle.com/datasets/blastchar/telco-customer-churn
- Licence: CC0 — Public Domain
- Rows: 7,043 customers
- Columns: 21 (original) + 3 derived

---

## Table: factSubscription
Grain: One row per customer — point-in-time snapshot.
This is not a transactional fact table. Each row represents
the full subscription history summary for one customer.

| Column | Data Type | Source | Business Meaning | Transform Notes |
|---|---|---|---|---|
| customerID | Text | Raw | Unique customer identifier. Primary key and foreign key to dimCustomer. | Retained as-is |
| tenure | Integer | Raw | Number of months the customer has been with the company. | Retained as-is. Used in CLV proxy and tenure band derivation. |
| Contract | Text | Raw | Contract type. Foreign key to dimContract. | Retained as-is. Values: Month-to-month, One year, Two year. |
| InternetService | Text | Raw | Type of internet service subscribed to. | Retained as-is. Values: DSL, Fiber optic, No. |
| MonthlyCharges | Decimal | Raw | Current monthly bill amount in USD. | Retained as-is. Used in MRR and CLV calculations. |
| TotalCharges | Decimal | Raw | Total amount charged over customer lifetime. | Null values replaced with 0 (new customers with no charges yet). Converted from text to decimal. |
| Churn | Text | Raw | Whether the customer has churned. | Retained as-is. Values: Yes, No. |
| c_IsChurned | Integer | Derived | Binary churn flag: 1 = churned, 0 = active. | Calculated in Power Query: if Churn = "Yes" then 1 else 0. |
| c_TenureBand | Text | Derived | Tenure grouped into 6 bands. Foreign key to dimTenureBand. | Calculated in Power Query using tenure ranges. Values: 01: 0-12M through 06: 60M+. |
| c_MRRRisk | Decimal | Derived | Monthly revenue at risk: MonthlyCharges if churned, else 0. | Calculated in Power Query: c_IsChurned × MonthlyCharges. |

---

## Table: dimCustomer
Grain: One row per unique customerID.
Built as a reference copy of factSubscription keeping demographic
and service configuration columns only.

| Column | Data Type | Source | Business Meaning | Transform Notes |
|---|---|---|---|---|
| customerID | Text | Raw | Primary key. Unique customer identifier. | Deduplicated on customerID. |
| gender | Text | Raw | Customer gender. | Retained as-is. Values: Male, Female. |
| SeniorCitizen | Text | Derived | Senior citizen status. | Converted from 0/1 integer to "Senior"/"Non-Senior" text label in Power Query. |
| Partner | Text | Raw | Whether customer has a partner. | Retained as-is. Values: Yes, No. |
| Dependents | Text | Raw | Whether customer has dependents. | Retained as-is. Values: Yes, No. |
| Contract | Text | Raw | Contract type. | Retained as-is. Foreign key to dimContract. |
| InternetService | Text | Raw | Internet service type. | Retained as-is. |
| PaymentMethod | Text | Raw | Payment method used. | Retained as-is. Values: Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic). |
| PaperlessBilling | Text | Raw | Whether customer uses paperless billing. | Retained as-is. Values: Yes, No. |

---

## Table: dimTenureBand
Grain: One row per tenure band.
Created manually via Enter Data in Power BI Desktop.

| Column | Data Type | Business Meaning | Notes |
|---|---|---|---|
| TenureBand | Text | Primary key. Tenure group label. | Values: 01: 0-12M through 06: 60M+. Prefix ensures correct sort order. |
| SortOrder | Integer | Numeric sort key for TenureBand. | Values: 1–6. Used to sort visuals correctly. |
| MinMonths | Integer | Minimum tenure months in this band. | Used for reference only. |
| MaxMonths | Integer | Maximum tenure months in this band. | 999 used for 60M+ band. |

---

## Table: dimContract
Grain: One row per contract type.
Created manually via Enter Data in Power BI Desktop.

| Column | Data Type | Business Meaning | Notes |
|---|---|---|---|
| Contract | Text | Primary key. Contract type label. | Values: Month-to-month, One year, Two year. |
| ContractType | Text | Classification of contract commitment level. | Values: Rolling, Fixed Term. |
| AnnualCommit | Text | Whether contract requires annual commitment. | Values: Yes, No. |

---

## Relationships

| From Table | From Column | To Table | To Column | Cardinality | Direction | Active |
|---|---|---|---|---|---|---|
| dimCustomer | customerID | factSubscription | customerID | 1:1 | Both | Yes |
| dimTenureBand | TenureBand | factSubscription | c_TenureBand | 1:* | Single | Yes |
| dimContract | Contract | factSubscription | Contract | 1:* | Single | Yes |

---

## Columns Excluded from Model

These columns exist in the raw dataset but were not loaded into
the final model. They are available in factSubscription for
ad-hoc analysis via slicers.

| Column | Reason |
|---|---|
| PhoneService | Low analytical value — subsumed by MultipleLines |
| MultipleLines | Standardised (No phone service → No) and kept in factSubscription |
| OnlineSecurity | Kept in factSubscription as slicer field |
| OnlineBackup | Kept in factSubscription as slicer field |
| DeviceProtection | Kept in factSubscription as slicer field |
| TechSupport | Kept in factSubscription as slicer field |
| StreamingTV | Kept in factSubscription as slicer field |
| StreamingMovies | Kept in factSubscription as slicer field |

---

## Known Data Quality Issues

| Issue | Impact | Resolution |
|---|---|---|
| TotalCharges contains nulls for new customers | Cannot calculate total spend for new customers | Replaced with 0 — documented in limitations |
| SeniorCitizen stored as 0/1 integer | Not human-readable in visuals | Converted to "Senior"/"Non-Senior" text in Power Query |
| No subscription dates available | Cannot build true time-series cohorts | Tenure bands used as proxy — documented in limitations |
| Point-in-time snapshot only | Cannot track customer behaviour over time | Single snapshot analysis only — noted in report |