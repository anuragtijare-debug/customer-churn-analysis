# Customer Churn Analysis (Python, SQL, Pandas, Seaborn)

End-to-end churn analysis for a subscription business: Excel -> SQLite -> cleaning -> merged table -> KPIs -> visuals -> insights.

> **Note:** Sample dataset of 21 customers across 3 tables. The goal is to show the pipeline and analytical approach; results are directional, not statistically significant.

## Data
`customer_churn_data_raw.xlsx`

| Sheet | Rows | Description |
|---|---|---|
| `db_customer` | 21 | name, country, state, gender, dob |
| `db_subscription` | 21 | plan, contract, charges, cancellation info, CLTV, churn score |
| `db_support` | 9 | complaints, escalation flag, CSAT score |

## Workflow
1. **Load and store:** read the Excel sheets with Pandas and load them into SQLite.
2. **Clean:** renamed `name` to `customer_name`; dropped unused columns (`interests`, `pincode`, `col_1`, `comment`); standardized gender (Men/Women to Male/Female); filled missing country from state; converted date columns.
3. **Feature engineering:** `churn_flag`, `tenure_days` (fixed snapshot date), `complaint_count`, `churn_risk` (low / med / high from churn score).
4. **Duplicates:** support table had several complaints per customer, so the latest complaint was kept and `complaint_count` added. Row count checked before and after merge.
5. **Merge:** subscription + customer + support (left joins on `customerid`).
6. **KPIs and visuals:** Matplotlib and Seaborn charts, correlation heatmap, pivot tables.
7. **SQL:** five basic queries (GROUP BY, WHERE, JOIN, ORDER BY) on the cleaned tables.

## Key Findings
| Metric | Value |
|---|---|
| Churn rate | 28.57% (6 of 21) |
| Retention rate | 71.43% |
| ARPU | 18.85 |
| Escalation rate | 19.05% |
| Avg complaints per customer | 0.43 |
| Monthly revenue lost | 73.94 |
| Monthly revenue at risk (active, med/high risk) | 34.98 |

- **Basic plan churns most:** 60% (3 of 5) vs Standard 22% (2 of 9) and Premium 14% (1 of 7).
- **Escalated complaints go with churn:** all 4 customers whose latest complaint was escalated churned (correlation 0.77).
- **Top cancellation reason:** switched to competitor (2 of 6); price, content, streaming quality and a forgotten trial one each.
- **Data note:** one Premium customer is charged 92.99 vs about 23 for others, likely an entry error; it lifts ARPU.

## Recommendations
1. Offer upgrade or discount paths to Basic-plan users before renewal.
2. Trigger retention outreach when a complaint is escalated or CSAT is low.
3. Contact the 2 active customers with churn score >= 50 first.

## Tech Stack
Python, Pandas, NumPy, SQLite, SQL, Matplotlib, Seaborn

## Files
- `Churn_analysis_project.ipynb`: full analysis with outputs
- `customer_churn_data_raw.xlsx`: source data
