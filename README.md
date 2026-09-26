# Bank Transaction Analytics: Fraud Detection & Spending Insights Lakehouse

An end-to-end data lakehouse pipeline built on Apache Spark and Delta Lake using the Medallion Architecture (Bronze, Silver, Gold) to ingest financial transactions, detect fraudulent activity patterns, and serve dimensional models to Power BI.

---

## Project Overview

* **Domain:** Financial Technology / Transaction Monitoring & Fraud Analytics
* **Platform Stack:** Databricks (Apache Spark), Delta Lake, Python / PySpark, Power BI
* **Course:** Data Analysis and Visualization — Semester Project (Phase 1)
* **Project Team:**
  * **Salman Ali** — Roll Number: `24L2542`
  * **Ahmad Khan** — Roll Number: `24L2541`

---

## Architecture: Medallion Pipeline

The lakehouse pipeline processes transactional data across three distinct tiers:

```text
       Raw Data (PaySim CSV)
                 │
                 ▼
┌───────────────────────────────────┐
│     Bronze Layer (Delta Lake)     │  <-- Raw Ingestion + Audit Metadata
│  - Ingestion Timestamp            │      (ingestion_timestamp, source_batch_id)
│  - Batch Tracking                 │
└─────────────────┬─────────────────┘
                  │
                  ▼
┌───────────────────────────────────┐
│     Silver Layer (Delta Lake)     │  <-- Cleansed, Deduplicated & Sanitized
│  - DecimalType(18,2) Casting      │  - Malformed Record Quarantine
│  - ISO UTC Timestamp Parsing      │  - Cryptographic Salted Hashing (SHA-256)
└─────────────────┬─────────────────┘
                  │
                  ▼
┌───────────────────────────────────┐
│      Gold Layer (Star Schema)     │  <-- Analytics & Business Intelligence
│  - fact_transaction               │  - dim_customer, dim_merchant, dim_date
│  - agg_fraud_rate                 │  - agg_monthly_spend
└─────────────────┬─────────────────┘
                  │
                  ▼
         Power BI Dashboards
