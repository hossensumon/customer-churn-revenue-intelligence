# Customer Churn Revenue Intelligence

> **A data analytics portfolio project focused on customer churn, subscription behavior, support experience, and revenue exposure in an OTT-style subscription business.**

## Overview

Customer churn can reduce recurring revenue and customer lifetime value. This project outlines an end-to-end analytics workflow for examining churn patterns across customer, subscription, and support data, with the aim of turning reported metrics into useful business questions and retention recommendations.

The proposed workflow combines SQL-based data extraction with Python data preparation, feature engineering, exploratory data analysis (EDA), KPI calculation, and visualization.

**Important:** The metrics below are transcribed from the supplied project report. The underlying raw dataset, SQL scripts, and Python notebook were not provided for independent reproduction. Treat the numbers as *reported results* until the analysis is rerun and validated.

## Project Objectives

- Establish a consistent baseline for churn and retention.
- Compare churn across subscription plans, contract types, geography, and time periods.
- Explore whether support escalations and customer satisfaction are associated with churn.
- Quantify recurring revenue exposure and customer lifetime value (CLTV) impact using clearly defined metrics.
- Identify customer segments for further investigation and retention experiments.
- Translate analytical findings into measurable business recommendations.

## Reported Key Findings

| Metric | Reported result |
|---|---:|
| Overall churn rate | 28.6% |
| Retention rate | 71.4% |
| Monthly-contract churn | 55.6% |
| Annual-contract churn | 8.3% |
| Average customer tenure | 1,451 days |
| ARPU | Rs 18.8 |
| Total revenue | 395 |
| Revenue loss attributed to churn | Approximately 74; another section reports $73.94/month |
| Reported revenue loss percentage | 18% |
| CLTV lost | 2,047 |
| Geographic pattern | Karnataka reported as the most affected state |
| Time pattern | September 2024 reported as the month with the most churn |

### Initial interpretation

- The report shows a substantial difference between monthly- and annual-contract churn rates. This is a priority for further investigation, not proof that contract type itself causes churn.
- The reported Karnataka and September 2024 patterns should be checked against comparable cohort sizes, data coverage, pricing changes, service issues, and data quality.
- Revenue and CLTV figures need clear currency, unit, aggregation-period, and cohort definitions before they are used for business decisions.

## Tools & Technologies

- **SQL** — relational data extraction and joins
- **Python** — analysis workflow
- **pandas** — data cleaning, transformation, and aggregation
- **NumPy** — numerical operations
- **SQLite / sqlite3** — database access
- **Matplotlib** — charting
- **Seaborn** — statistical visualization

## Data Model

The supplied project brief describes a database named `customer_churn` containing three tables connected through `customerid`.

| Table | Fields described in the report | Purpose |
|---|---|---|
| `db_customer` | `customerid`, `name`, `country`, `state`, `gender`, `dob`, `interests`, `pincode` | Customer profile and geographic segmentation |
| `db_subscription` | `customerid`, `subscription_start_date`, `subscription_type`, `renewal_date`, `plan_type`, `contract_type`, `cancellation_date`, `cancellation_reason`, `monthly_charges`, `cltv`, `churn_score` | Subscription lifecycle, plan and contract analysis, churn, and revenue |
| `db_support` | `customerid`, `complaint_date`, `Escalations`, `csat_score`, `col_1`, `comment` | Support experience and satisfaction analysis |

Actual column names, capitalization, key uniqueness, and table grain should be confirmed against the database before running queries.

## Analytical Workflow

1. **Audit the data**  
   Inspect schema, row counts, join keys, missing values, duplicates, date ranges, and category consistency.

2. **Extract relational data**  
   Use SQL and Python's `sqlite3` to retrieve the required customer, subscription, and support information.

3. **Prepare and validate records**  
   Standardize data types, parse dates, review invalid values, and establish a consistent churn flag and analysis cut-off date.

4. **Engineer features**  
   Derive tenure, contract and plan segments, support metrics, and documented revenue fields where supported by the data.

5. **Perform exploratory analysis**  
   Compare churn by contract type, plan, geography, month, tenure, cancellation reason, and support experience.

6. **Calculate KPIs**  
   Use consistent definitions, reporting periods, populations, and denominators.

7. **Visualize and communicate**  
   Present trends and segment comparisons, then turn validated findings into prioritized business recommendations.

## KPI Framework

| KPI | Recommended definition |
|---|---|
| Churn rate | Customers who churned during the period ÷ customers eligible to churn during the period × 100 |
| Retention rate | 1 − churn rate, when the population and period align |
| Churn by segment | Churn rate calculated separately for each plan, contract type, or geographic segment |
| ARPU | Recurring revenue for a defined period ÷ a clearly defined active-customer denominator |
| Average tenure | Average time from subscription start to cancellation for churned customers; use a fixed cut-off for active customers |
| Revenue at risk | Recurring charges for customers meeting a documented risk rule |
| Escalation rate | Escalated support cases ÷ total support cases × 100 |
| Complaints per customer | Complaint records ÷ distinct customers in the defined population |
| Escalations and churn | Compare churn rates for customers with at least one escalation against those with none |

**KPI quality matters:** define the churn event, eligible population, observation window, currency, and time unit. Aggregate one-to-many support records before joining them to subscription data, or duplicated rows may inflate revenue totals. Revenue at risk is exposure; it is not the same as realized revenue loss.

## Business Recommendations to Investigate

1. **Investigate monthly-contract churn.** Segment by plan, tenure, acquisition cohort, price changes, and cancellation reason before proposing annual-plan migration or retention offers.
2. **Review high-risk customers.** Use a validated risk score together with customer value and support history; evaluate retention actions against a suitable control group.
3. **Analyze support experience.** Compare churn by escalation and CSAT history while accounting for tenure and plan where possible.
4. **Validate geographic and time patterns.** Investigate the reported Karnataka and September 2024 patterns for pricing, product, service, technical, cohort, or data-quality explanations.
5. **Use cancellation reasons.** Connect documented cancellation reasons to product, pricing, and content reviews, then measure changes after any intervention.

These are hypotheses for investigation. Observed relationships alone do not establish causation or guarantee that a proposed intervention will reduce churn.

## Suggested Repository Structure

```text
customer_churn_revenue_intelligence/
├── data/
│   └── README.md                 # Dataset source and data dictionary
├── sql/
│   └── churn_analysis.sql        # SQL extraction and aggregation queries
├── notebooks/
│   └── churn_analysis.ipynb      # Cleaning, EDA, and KPI calculations
├── outputs/
│   ├── charts/                   # Exported visualizations
│   └── tables/                   # KPI tables and analysis outputs
├── reports/
│   └── Professional_Customer_Churn_Analysis_Report.pdf
├── requirements.txt
└── README.md
```

This is a suggested layout. Keep only folders and files that actually exist in the published repository, and update filenames as needed. Do not publish private customer information; use authorized, anonymized, or appropriately synthetic data.

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd customer_churn_revenue_intelligence
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

If `requirements.txt` is included:

```bash
pip install -r requirements.txt
```

Otherwise, install the main analysis libraries:

```bash
pip install pandas numpy matplotlib seaborn
```

SQLite support is generally included with Python through `sqlite3`.

### 4. Run the analysis

Prepare the authorized database or dataset, update the database path in the code if necessary, and run the SQL scripts or notebook included in the repository. The exact command depends on the actual files published.

## Limitations & Validation Checklist

Before presenting the findings as verified results:

- [ ] Reproduce all headline metrics from the underlying data and code.
- [ ] Define the churn flag, analysis cut-off date, eligible population, and reporting period.
- [ ] Verify key uniqueness and table grain; prevent duplicated charges after joins.
- [ ] Confirm the meaning and scale of `churn_score`, `cltv`, and `col_1`.
- [ ] Reconcile revenue figures, including the reported value of 74 and the separate $73.94/month figure.
- [ ] Document currency, units, and time period for total revenue, ARPU, revenue loss, and CLTV.
- [ ] Validate segment sizes and denominators for monthly/annual contract comparisons.
- [ ] Describe associations as associations unless a suitable causal design supports stronger conclusions.
- [ ] Avoid claiming a deployed predictive model, live-data analysis, or achieved business impact unless those activities were completed and can be demonstrated.

## Skills Demonstrated

- SQL querying and relational data modeling
- Python-based data preparation and analysis
- Data quality checks and feature engineering
- Exploratory data analysis
- KPI definition and calculation
- Customer segmentation and churn analysis
- Revenue exposure and CLTV analysis
- Data visualization and business communication
- Evidence-based recommendations and analytical validation

## Project Summary

**Customer Churn Revenue Intelligence** connects customer, subscription, and support analytics to investigate churn behavior and potential revenue exposure. Its most prominent reported signal is the difference between monthly- and annual-contract churn. Reproducing the metrics, documenting definitions, reconciling revenue units, and validating joins are essential next steps for making the project reproducible and interview-ready.

## Source

This README is based on the supplied *Professional Customer Churn Analysis Report* and the project brief it reviews. Numerical findings are presented as reported results because the raw dataset and executable analysis were not included for independent verification.

