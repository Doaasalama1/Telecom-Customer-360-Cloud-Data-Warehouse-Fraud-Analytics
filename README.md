# Telecom Customer 360 – Cloud Data Warehouse & Fraud Analytics

An end-to-end Data Engineering project that detects and classifies harmful calling behavior such as **Vishing (Voice Phishing)** and **Telephone Harassment** using a transparent, explainable rule-based fraud scoring engine.

The platform processes **1.4 million call records** for **5,000 telecom customers** over a **3-month period** and generates a **Fraud Score (0–100)** to classify customers into risk levels.

## Project Objective

Build a scalable cloud data platform that:

* Processes telecom customer data at scale.
* Detects suspicious calling behavior.
* Generates explainable fraud scores.
* Provides actionable business insights through dashboards.
* Implements a complete Medallion Architecture pipeline.

---

## Risk Classification

| Risk Level  | Score Range | Action                           |
| ----------- | ----------- | -------------------------------- |
| 🔴 Critical | 80 – 100    | Immediate investigation required |
| 🟠 High     | 60 – 79     | High probability – investigate   |
| 🟡 Medium   | 30 – 59     | Needs monitoring                 |
| 🟢 Low      | 0 – 29      | Normal behavior                  |

> Every fraud score is fully explainable and can be traced back to the exact rule that triggered it.

---

# Architecture

```
Raw CSV Files
       │
       ▼
Azure Data Lake Storage Gen2
(Bronze Layer)
       │
       ▼
Azure Databricks (PySpark)
(Silver Layer)
       │
       ▼
Feature Engineering + Fraud Engine
(Gold Layer)
       │
       ▼
Azure SQL Database
(Star Schema)
       │
       ▼
Power BI Dashboard
```

---

# Medallion Architecture

##  Bronze Layer

Raw data ingestion.

Sources:

* Calls
* Customers
* Recharges
* Complaints

Storage:

* Azure Data Lake Storage Gen2

---

##  Silver Layer

Data cleaning and validation.

Tasks:

* Data quality checks
* Deduplication
* Missing values handling
* Data validation

Output:

* Parquet files

Applied:

* 13 Data Quality Rules

---

##  Gold Layer

Feature engineering and fraud analysis.

Outputs:

* Customer behavioral features
* Fraud score (0–100)
* Risk classification

---

# Technology Stack

| Layer           | Technology                   | Purpose             |
| --------------- | ---------------------------- | ------------------- |
| Storage         | Azure Data Lake Storage Gen2 | Raw & cleaned data  |
| Processing      | Azure Databricks (PySpark)   | Data transformation |
| Data Warehouse  | Azure SQL Database           | Star Schema         |
| Fraud Engine    | SQL Rule-Based Engine        | Fraud detection     |
| Visualization   | Power BI                     | Dashboards          |
| Data Generation | Python (Pandas, NumPy)       | Simulated dataset   |

---

# Fraud Rules Engine

## 🔴 Negative Indicators

| Rule                  | Description                              | Weight |
| --------------------- | ---------------------------------------- | ------ |
| High Call Frequency   | > 60 calls/day                           | +20    |
| Very Short Calls      | Avg duration < 10 sec                    | +15    |
| Repeated Calls        | > 10 calls/hour to same number           | +15    |
| Random Number Pattern | > 20 new numbers/day + answer rate < 20% | +20    |
| Complaints            | Fraud/Harassment complaints              | +30    |
| High Off-net Usage    | Off-net > 60% + complaints               | +15    |

---

## 🟢 Positive Indicators

| Rule                     | Description                | Weight |
| ------------------------ | -------------------------- | ------ |
| Number Correction        | 1–2 digit difference       | -5     |
| Normal Call Duration     | 60–300 sec                 | -10    |
| Regular Recharge Pattern | Consistent weekly behavior | -10    |

---

# Fraud Score Formula

```text
Fraud Score =
(Sum of Negative Rules)
-
(Sum of Positive Rules)

Fraud Score = max(0, min(100, Fraud Score))
```

---

# Project Results

| Metric                       | Value       |
| ---------------------------- | ----------- |
| Calls Processed              | 1.4 Million |
| Customers                    | 5,000       |
| Fraud Rules                  | 9           |
| High/Critical Risk Customers | 115         |
| Data Quality                 | 98.8%       |
| Runtime                      | ~15 Minutes |
| Idempotency                  | Verified    |

---

# Risk Distribution

```text
🟢 Low       65%
🟡 Medium    25%
🟠 High       8%
🔴 Critical   2%

Total Customers: 5,000
```

---

# Data Warehouse Design

Star Schema:

### Fact Table

* fact_events_dedup

### Dimension Tables

* dim_customer
* dim_date
* dim_location
* dim_plan
* dim_subscription
* dim_recharge
* dim_complaint
* dim_risk

---

# Power BI Dashboard

## Executive Dashboard

* Total Calls
* Revenue
* Fraud Rate
* Active Customers
* Revenue Trend

## Fraud Analysis

* Top 5 High-Risk Customers
* Risk Distribution
* Behavioral Patterns

## Customer Behaviour

* Age Distribution
* Subscription Type
* Average Call Duration
* Recharge Channels

## Complaints Analytics

* Total Complaints
* Resolution Days
* Complaint Categories
* Monthly Trend

---

# Repository Structure

```text
Telecom-Customer-360/

├── bronze/
├── silver/
├── gold/

├── scripts/
├── notebooks/
├── reports/
├── docs/

└── README.md
```

---

# Team

| Member | Role                | Contribution                              |
| ------ | ------------------- | ----------------------------------------- |
| Doaa   | Presentation Lead   | Storytelling, Business Value, Q&A         |
| Wesam  | Data Engineer       | Medallion Pipeline, PySpark, Fraud Engine |
| Esraa  | BI & Data Warehouse | Star Schema, Power BI, Documentation      |

---

# Future Improvements

* Machine Learning fraud detection.
* Real-time streaming using Azure Event Hub.
* Automated alerts for Critical Risk customers.
* CI/CD deployment pipeline.

---

**Data Engineering Capstone Project — June 2026**
