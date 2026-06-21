![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboarding-F2C811?logo=powerbi)
![License](https://img.shields.io/badge/License-MIT-green)

# US Robotaxi Safety Intelligence & Incident Analytics

### An operational intelligence project analyzing real-world autonomous vehicle safety incidents, deployment risk patterns, and reporting behavior across the U.S. robotaxi ecosystem.

---

# Executive Summary

This project analyzes real-world Autonomous Driving System (ADS) crash reports submitted to the U.S. National Highway Traffic Safety Administration (NHTSA) between June 2025 and April 2026.

Using Python for data engineering and exploratory analysis, and Power BI for executive-level intelligence reporting, the project investigates operational safety patterns, deployment concentration, injury outcomes, reporting compliance, and interaction risks across major autonomous vehicle operators.

The analysis began with **895 raw incident reports across 116 fields** and produced a cleaned canonical dataset of **861 incidents and 23 analysis-ready features** after duplicate reconciliation, feature engineering, category normalization, and data quality validation.

Rather than asking *"Which company is safest?"*, the project focuses on a more defensible operational question:

> What risk patterns, deployment characteristics, and reporting behaviors emerge from real-world robotaxi operations?

---

# Business Problem

Commercial autonomous vehicle deployments are expanding rapidly across U.S. cities, yet public discussions around robotaxi safety often focus on isolated incidents rather than systematic operational patterns.

Regulators, operators, and transportation stakeholders require a structured understanding of:

- Where incidents occur
- Which environments create the most operational complexity
- How injury severity varies by interaction type
- Whether reporting behavior differs across operators
- How deployment expansion influences observed incident volumes

This project transforms raw regulatory filings into actionable operational intelligence through data cleaning, exploratory analysis, and business-focused dashboarding.

---

# Dataset

**Source:** NHTSA ADS Crash Reporting Dataset

Coverage Period:

- June 2025 – April 2026

Original Dataset:

| Metric | Value |
|----------|---------|
| Rows | 895 |
| Columns | 116 |
| Reporting Entities | 13 |
| Dataset Type | Regulatory ADS Incident Reports |

Major Operators Included:

- Waymo LLC
- Tesla, Inc.
- Zoox, Inc.
- Avride Inc.
- May Mobility
- Motional
- Aurora Operations
- Nuro
- Hyundai Motor America
- Additional operators

Core analytical fields included:

- Report ID
- Reporting Entity
- Incident Date
- Report Submission Date
- City / State
- Roadway Type
- Crash With
- Highest Injury Severity Alleged
- Automation System Engaged
- Engagement Status
- Within ODD
- Vehicle Speed
- Weather Conditions
- Narrative Text

### Important Dataset Limitations

- Dataset contains month/year dates only (no day-level granularity)
- 2026 represents partial-year coverage
- April 2026 is incomplete
- No exposure metrics exist (miles driven, fleet size, trips, operational hours)

As a result:

> Incident volume should not be interpreted as safety performance.

Higher incident counts may reflect:

- Larger deployment footprints
- Greater operational exposure
- Higher reporting maturity
- Increased fleet activity

---

# Methodology

The project followed a structured analytics workflow:

### Phase 1 — Data Understanding

- Dataset profiling
- Schema review
- Column selection
- Data quality assessment

### Phase 2 — Data Cleaning

- Missing value validation
- Date parsing
- Duplicate investigation
- Category normalization

### Phase 3 — Feature Engineering

Created:

- Incident Year
- Incident Month
- Incident Month Name
- Incident Quarter
- Reporting Delay Months
- Weather Condition
- Severity Group
- Crash Counterpart Group

### Phase 4 — Exploratory Analytics

Analysis domains:

- Severity intelligence
- Geographic concentration
- Deployment scaling
- Reporting compliance
- Operator comparisons
- Roadway risk analysis
- Counterpart interaction analysis

### Phase 5 — BI Storytelling

Interactive Power BI dashboard designed around:

- Executive Intelligence
- Operational Risk
- Geographic Intelligence
- Reporting Compliance

---

# Data Cleaning

The dataset required significant preparation before analysis.

### Missing Values

Dataset quality was unusually strong:

| Field | Missing Values |
|---------|---------|
| City | 2 |
| State | 2 |
| All Other Core Fields | 0 |

### Duplicate Investigation

The dataset behaved more like a **living regulatory reporting system** than a static incident log.

Findings:

- Exact duplicate rows: 1
- Duplicate Report IDs: 34

Observed duplicate patterns:

- Severity escalations
- Narrative de-redaction
- Regulatory corrections
- ADS/ADAS reclassification
- Staged report amendments

Deduplication policy:

```text
Keep latest Report Submission Date per Report ID
```

Final Result:

| Stage | Rows |
|---------|---------|
| Original | 895 |
| Removed | 34 |
| Final Canonical Dataset | 861 |

---

# Feature Engineering

### Weather Consolidation

Original fields:

- Weather - Clear
- Weather - Rain
- Weather - Fog/Smoke/Haze

Converted into:

```text
Weather Condition
├── Clear
├── Rain
└── Unknown
```

Distribution:

| Weather | Count |
|-----------|---------|
| Clear | 680 |
| Unknown | 177 |
| Rain | 38 |

### Severity Normalization

Raw injury categories were consolidated into:

- No Injury
- Minor Injury
- Moderate Injury
- Fatality
- Unknown

### Crash Counterpart Grouping

Created higher-level operational categories:

- Passenger Vehicle
- Commercial Vehicle
- Vulnerable Road User
- Two-Wheeler
- Fixed Infrastructure
- Animal
- Emergency Vehicle
- Other / Unknown

---

# Key Findings

### 1. Most Incidents Are Low Severity

- ~88% categorized as No Injury
- Only one fatality recorded
- Moderate injuries were extremely rare

**Insight:** Most robotaxi incidents are low-energy operational interaction events rather than catastrophic failures.

---

### 2. Vulnerable Road Users Show Disproportionately Higher Severity

VRU incidents represented only:

- 1.39% of incidents

But exhibited:

- 58% Minor Injury
- 8% Moderate Injury

**Insight:** VRU interactions are rare but significantly more severe when they occur.

---

### 3. Deployment Is Highly Concentrated

Top states:

| State | Share |
|---------|---------|
| California | 54.71% |
| Texas | 18.51% |
| Arizona | 16.07% |

Nearly 89% of incidents originated from only three states.

---

### 4. Incident Growth Reflects Deployment Scaling

Monthly incidents increased from approximately:

```text
~60 incidents/month
        ↓
~118 incidents/month
```

Interpretation:

- Fleet expansion
- Geographic rollout
- Increased operational exposure

Not deteriorating safety.

---

### 5. Reporting Compliance Is Highly Mature

Reporting delay distribution:

| Delay | Count |
|---------|---------|
| 0 Months | 405 |
| 1 Month | 463 |
| 2+ Months | 27 |

**97% of incidents were reported within 0–1 months.**

---

# Dashboard Pages

The Power BI solution was intentionally designed as an operational intelligence product rather than a generic reporting dashboard.

## Page 1 — Executive Overview

Purpose:

> What is happening overall?

KPIs:

- Canonical Incidents
- Active Operators
- No Injury %
- Injury Related %
- Deployment Cities
- Average Reporting Delay

Key Visuals:

- Monthly Incident Trend (Hero Visual)
- Severity Treemap
- Bubble City Map
- Top Operators
- Dynamic Intelligence Panel

---

## Page 2 — Operational Risk Analysis

Purpose:

> Where does injury risk occur?

KPIs:

- Injury %
- Minor Injury %
- Severe %
- VRU %

Key Visuals:

- Severity by Crash Counterpart Group
- Severity by Roadway Type
- VRU Severity Treemap
- Severity by Weather Condition

---

## Page 3 — Geographic & Operator Intelligence

Purpose:

> Where are deployments concentrated?

KPIs:

- Active Operators
- Deployment Cities
- Top State Share %
- Top Operator Share %

Key Visuals:

- State Deployment Map
- Top Cities
- Operator Market Share Treemap
- Geographic Footprint Analysis

---

## Page 4 — Reporting Compliance & Operational Maturity

Purpose:

> How quickly are incidents reported?

KPIs:

- Same Month Reporting %
- Reported Within 1 Month %
- Average Delay
- Long Delay %

Key Visuals:

- Reporting Delay Distribution
- Severity Composition by Delay
- Reporting Behavior by Operator
- Long-Tail Delay Spotlight

---

## Standout DAX & BI Components

- Dynamic Insight Cards
- Custom Tooltip Pages
- Severity Mix Tooltips
- Reporting Delay Intelligence
- KPI Variance Measures
- Time Intelligence Measures
- Custom Date Table
- Dedicated Measures Table

---

# Tools Used

| Category | Tools |
|-----------|---------|
| Programming | Python 3.13 |
| Data Processing | pandas, numpy |
| Visualization | matplotlib, seaborn |
| Business Intelligence | Power BI Desktop |
| Modeling | DAX |
| Data Preparation | Power Query |
| Version Control | Git & GitHub |
| Future ML | scikit-learn (planned) |

---

# Project Structure

```text
robotaxi-analytics/
│
├── README.md
│
├── data/
│   ├── raw/
│   │   └── Original NHTSA ADS dataset
│   │
│   └── processed/
│       └── Canonical cleaned dataset
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_cleaning.ipynb
│   └── 03_operational_analysis.ipynb
│
├── reports/
│   ├── Robotaxi_Incident_Analysis_Repot.docx
│   └── Duplicate Investigation Report
│
├── powerbi/
│   ├── robotaxi_dashboard.pbit
│   ├── model.tmdl
│   ├── relationships.tmdl
│   ├── database.tmdl
│   ├── DateTable.tmdl
│   ├── robotaxi_cleaned.tmdl
│   └── _measures table.tmdl
│
├── visuals/
│   ├── severity_analysis/
│   ├── geography_analysis/
│   ├── reporting_delay_analysis/
│   └── operator_trends/
│
├── sql/
│   └── future_sql_analysis.sql
│
└── notes/
    ├── methodology_notes.md
    ├── data_quality_log.md
    └── analytical_findings.md
```

### Repository Assets

Power BI Template:

```text
/powerbi/robotaxi_dashboard.pbit
```

Analytical Reports:

```text
/reports/
```

Python Analysis:

```text
/03_operational_analysis.ipynb
```

---

# Future Enhancements

### Planned v2 Enhancements

- NLP analysis of incident narratives
- Topic modeling
- Operator comparison scorecards
- SQL analytics layer
- Automated data refresh pipeline
- Predictive severity modeling using scikit-learn
- Advanced geospatial analysis
- Executive KPI benchmarking

---

# Screenshots

## Executive Overview

![Executive Overview](images/executive-overview.png)

## Operational Risk Analysis

![Operational Risk Analysis](images/operational-risk.png)

## Geographic & Operator Intelligence

![Geographic Intelligence](images/geographic-intelligence.png)

## Reporting Compliance Dashboard

![Reporting Compliance](images/reporting-compliance.png)

---

# Skills Demonstrated

### Data Analytics

- Exploratory Data Analysis (EDA)
- Data Quality Assessment
- Duplicate Reconciliation
- Feature Engineering
- Statistical Summarization

### Python

- pandas
- numpy
- matplotlib
- seaborn

### Business Intelligence

- Power BI Dashboard Design
- DAX Measure Development
- Data Modeling
- Interactive Reporting
- KPI Design

### Analytical Thinking

- Operational Risk Analysis
- Transportation Safety Analytics
- Regulatory Reporting Analysis
- Deployment Intelligence
- Executive Storytelling

### Professional Practices

- GitHub Documentation
- Reproducible Analytics Workflow
- Data Governance
- Assumption Documentation
- Business-Oriented Communication

---

# References / Citations

### Data Sources

1. National Highway Traffic Safety Administration (NHTSA)
   - ADS Crash Reporting Dataset
   - https://www.nhtsa.gov/laws-regulations/standing-general-order-crash-reporting

2. U.S. Consumer Product Safety Commission (CPSC)
   - Public safety and incident reporting resources
   - https://www.cpsc.gov

### Disclaimer

This project is intended for analytical and educational purposes. Incident counts should not be interpreted as direct measures of autonomous vehicle safety performance because exposure metrics (miles driven, fleet size, trip volume, operational hours) are not available in the source dataset.
