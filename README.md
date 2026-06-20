# Circle B2B Sales Analysis

## 📌 Project Overview
This project analyzes Circle's B2B sales funnel data using SQL on BigQuery.
Circle is a French sustainable sportswear brand that sells part of its production to B2B customers.
The goal is to understand the current state of the sales funnel and calculate key conversion KPIs.

## 🎯 Objectives
- Explore and understand the cc_funnel dataset
- Identify the primary key and fix data anomalies
- Enrich the data by adding a deal_stage column
- Analyze funnel performance globally, by priority, and by month

## 🗂️ Dataset
**Table:** `cc_funnel` (188 records after cleaning)

| Column | Description |
|---|---|
| company | Company name (primary key) |
| sector | Business sector |
| date_lead | Date the prospect entered the funnel |
| date_opportunity | Date the prospect became an opportunity (after demo) |
| date_customer | Date the prospect signed a contract |
| date_lost | Date the prospect was lost |
| priority | Prospect importance level (High / Medium / Low) |

## 🔧 Methodology

### Step 1 - Data Exploration
- Inspected table structure: 7 columns, 998 rows
- Found 810 NULL rows (import anomaly from Google Sheets)
- Identified 188 valid records
- Confirmed `company` as the primary key

### Step 2 - Data Enrichment
Created `cc_funnel_kpi` table with a new `deal_stage` column using CASE WHEN logic:
- `date_customer` is not null → **3-Customer**
- `date_lost` is not null → **4-Lost**
- `date_opportunity` is not null → **2-Opportunity**
- Otherwise → **1-LEAD**

### Step 3 - Funnel Overview
| Deal Stage | Count |
|---|---|
| 1-LEAD | 79 |
| 2-Opportunity | 33 |
| 3-Customer | 23 |
| 4-Lost | 53 |

### Step 4 - Funnel KPIs

**Global:**
| KPI | Value |
|---|---|
| L2O (Lead to Opportunity) | 41.0% |
| L2C (Lead to Customer) | 12.2% |
| O2W (Opportunity to Win) | 29.9% |
| Avg days Lead to Opportunity | 12.2 days |
| Avg days Lead to Customer | 18.8 days |
| Avg days Opportunity to Customer | 8.1 days |

**By Priority:**
| Priority | L2O | L2C | O2W |
|---|---|---|---|
| High | 49.5% | 16.5% | 33.3% |
| Medium | 34.0% | 10.0% | 29.4% |
| Low | 31.9% | 6.4% | 20.0% |

**By Month:**
| Month | nb_lead | L2O | L2C |
|---|---|---|---|
| 2022-05 | 15 | 73.3% | 20.0% |
| 2022-06 | 22 | 68.2% | 18.2% |
| 2022-07 | 78 | 26.9% | 11.5% |
| 2022-08 | 73 | 41.1% | 9.6% |

## 💡 Key Insights
1. **Quality over quantity** — May-June had fewer but higher quality leads (L2O 73%), July-August had more but lower quality
2. **Main bottleneck is post-demo** — O2W is only 29.9%, demo quality should be improved
3. **Focus on High priority companies** — L2C is 16.5% vs only 6.4% for Low priority
4. **Sales cycle is fast** — Average 18.8 days from lead to customer

## 🛠️ SQL Techniques Used
- `COUNT`, `COUNT(DISTINCT)` — row and unique value counting
- `COUNTIF` — conditional counting
- `CASE WHEN` — conditional value assignment
- `GROUP BY` / `HAVING` — grouping and filtering
- `DATE_DIFF` — calculating days between dates
- `FORMAT_DATE` — formatting dates to year-month
- `ROUND` — decimal rounding
- `CREATE TABLE AS` — creating new table from query
- `WHERE IS NOT NULL` — filtering null values

## 🗃️ Tools
- Google BigQuery (SQL)
- Google Sheets (data source)
- GitHub (version control)
