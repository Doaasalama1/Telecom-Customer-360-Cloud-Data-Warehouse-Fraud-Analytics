# Telecom Customer 360 – Cloud Data Warehouse & Fraud Analytics


##  Project Overview

**Telecom Customer 360** is an end-to-end data engineering pipeline designed to detect and classify **harmful calling behavior** – including **vishing (voice phishing)** and **telephone harassment** – using a transparent, rule-based scoring engine.

The platform processes **1.4 million call records** for **5,000 customers** in **Nasr City** over **3 months**, applying **9 behavioral rules (6 Negative + 3 Positive)** to generate a **Fraud Score (0–100)** and classify customers into **4 risk levels**:

| Risk Level | Score Range | Action Required |
|------------|-------------|-----------------|
| 🔴 **Critical** | 80 – 100 | Immediate investigation required |
| 🟠 **High** | 60 – 79 | High probability – investigate |
| 🟡 **Medium** | 30 – 59 | Suspicious – needs monitoring |
| 🟢 **Low** | 0 – 29 | Normal behavior – no action needed |

> **Core Innovation:** Fully explainable fraud detection – every score can be traced back to the exact rule that triggered it.

---

##  System Architecture (Medallion)
┌─────────────────────────────────────────────────────────────────┐
│ BRONZE LAYER (Raw Data) │
│ • Raw CSV files (calls, recharges, complaints, customers) │
│ • Azure Data Lake Storage Gen2 │
└─────────────────────────────────────────────────────────────────┘
│
▼ (Azure Databricks – PySpark)
┌─────────────────────────────────────────────────────────────────┐
│ SILVER LAYER (Cleaned) │
│ • 13 data quality rules applied │
│ • Deduplication & validation │
│ • Output: Parquet files │
└─────────────────────────────────────────────────────────────────┘
│
▼ (Azure Databricks – Feature Engineering)
┌─────────────────────────────────────────────────────────────────┐
│ GOLD LAYER (Aggregated) │
│ • Feature engineering per customer │
│ • 9-rule Fraud Scoring Engine │
│ • Fraud Score (0–100) & Risk Level │
└─────────────────────────────────────────────────────────────────┘
│
▼ (Azure SQL Database)
┌─────────────────────────────────────────────────────────────────┐
│ STAR SCHEMA (Data Warehouse) │
│ • 1 Fact Table (fact_events_dedup) │
│ • 8 Dimension Tables (customer, date, location, plan, etc.) │
└─────────────────────────────────────────────────────────────────┘
│
▼ (Power BI)
┌─────────────────────────────────────────────────────────────────┐
│ 4-PAGE INTERACTIVE DASHBOARD │
│ 1. Executive Dashboard │ 2. Fraud Analysis │
│ 3. Customer Behaviour │ 4. Complaints Analytics │
└─────────────────────────────────────────────────────────────────┘

text

---

##  Technology Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| **Data Lake** | Azure Data Lake Storage Gen2 | Raw & cleaned data storage |
| **Processing** | Azure Databricks (PySpark) | Distributed data transformation |
| **Data Warehouse** | Azure SQL Database | Star Schema (1 Fact + 8 Dims) |
| **Fraud Logic** | Rule-Based Engine (SQL) | 9 rules: 6 Negative + 3 Positive |
| **Visualization** | Power BI Desktop & Service | 4-page interactive dashboard |
| **Data Generation** | Python (pandas, NumPy) | Simulated dataset (5,000 customers) |

---

##  Fraud Rules Engine

### 🔴 Negative Indicators (Increase Score)

| # | Rule | Condition | Weight |
|---|------|-----------|--------|
| 1 | High Call Frequency | > 60 calls per day | +20 |
| 2 | Very Short Calls | Average duration < 10 seconds | +15 |
| 3 | Repeated Calls | > 10 calls to same number in 1 hour | +15 |
| 4 | Random Number Pattern | > 20 new numbers/day + answer rate < 20% | +20 |
| 5 | Complaints | Any complaint (fraud/harassment/scam) | +30 |
| 6 | High Off-net + Complaints | Off-net > 60% AND complaints exist | +15 |

### 🟢 Positive Indicators (Decrease Score)

| # | Rule | Condition | Weight |
|---|------|-----------|--------|
| 7 | Number Correction | Same number with 1-2 digit difference | -5 |
| 8 | Normal Call Duration | Average duration between 60–300 seconds | -10 |
| 9 | Regular Recharge Pattern | Consistent recharge day & amount (weekly routine) | -10 |

### 📈 Risk Score Calculation
Risk Score = (sum of negative weights) - (sum of positive weights)
Risk Score = max(0, min(100, Risk Score))

text

---

##  Key Results

| Metric | Value |
|--------|-------|
| **Calls Processed** | 1.4 Million |
| **Customers Profiled** | 5,000 |
| **Fraud Rules** | 9 (6 Negative + 3 Positive) |
| **High/Critical Risk Customers** | 115 |
| **Data Quality** | 98.8% (after 13 cleaning rules) |
| **Pipeline Runtime** | ~15 minutes |
| **Idempotency** | Verified (3 re-runs – identical results) |

### Risk Distribution
┌─────────────────────────────────────────────────────────────────┐
│ Risk Distribution │
│ │
│ 🟢 Low ██████████████████████████████████████████ 65% │
│ 🟡 Medium ████████████████████████████ 25% │
│ 🟠 High ████████████████ 8% │
│ 🔴 Critical ████ 2% │
│ │
│ Total: 5,000 Customers │
└─────────────────────────────────────────────────────────────────┘

text

---

##  Power BI Dashboard (4 Pages)

### 1. Executive Dashboard
- KPIs: Total Calls, Revenue, Fraud Rate, Active Customers
- Fraud Rate Gauge
- Revenue by Plan (Bar Chart)
- Monthly Revenue Trend (Line Chart)

### 2. Fraud Analysis
- Top 5 High-Risk Customers (Table)
- Risk Distribution (Donut Chart)
- Behavioral Patterns Detected
- Risk Level Slicer

### 3. Customer Behaviour
- Age Distribution
- Subscription Type (Prepaid/Postpaid)
- Avg Call Duration by Plan
- Recharge Channel Popularity

### 4. Complaints Analytics
- Total Complaints, Avg Resolution Days, Open Complaints
- Complaints by Category
- Complaints by Month (Trend)
- Severity Slicer

---

##  Team

| Member | Role | Key Contributions |
|--------|------|-------------------|
| **Doaa** | Presentation Lead | Storytelling, business value framing, Q&A |
| **Wesam** | Data Engineering | Medallion pipeline, PySpark, fraud rules engine |
| **Esraa** | Data Warehousing & BI | Star Schema, Power BI dashboard, KPIs, documentation |

---

##  Repository Structure
Telecom-Customer-360/
├── bronze/ # Raw CSV data
├── silver/ # Cleaned Parquet files
├── gold/ # Aggregated features & fraud scores
├── scripts/ # Python scripts (data generation, cleaning)
├── notebooks/ # Databricks notebooks
├── reports/ # Data quality reports
├── docs/ # Architecture diagrams




---

**Made with ❤️ as a Data Engineering Capstone Project – June 2026**
