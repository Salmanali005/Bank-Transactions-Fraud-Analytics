# Consumer Financial Complaint Analytics Lakehouse

An end-to-end data lakehouse pipeline built on Apache Spark and Delta Lake using the Medallion Architecture (Bronze, Silver, Gold) to ingest real consumer complaint data from the CFPB, analyze complaint and resolution patterns, and serve dimensional models to Power BI.

---

## Project Overview

* **Domain:** Financial Services / Consumer Complaint & Risk Analytics
* **Data Source:** [CFPB Consumer Complaint Database API](https://www.consumerfinance.gov/data-research/consumer-complaints/) — real, live, publicly available complaint records against financial companies, updated daily by the U.S. government
* **Platform Stack:** Databricks (Apache Spark), Delta Lake, Python / PySpark, Power BI
* **Course:** Data Analysis and Visualization — Semester Project (Phase 1)
* **Project Team:**
  * **Salman Ali** — Roll Number: `24L2542`
  * **Ahmad Khan** — Roll Number: `24L2541`

---

## Why This Data Source

Unlike a static synthetic dataset, the CFPB API provides genuinely live data: a **Full Load** is pulled as a historical baseline over a fixed date range, and an **Incremental Load** is pulled separately for complaints received after that cutoff — mirroring how a real production pipeline would ingest new records as they arrive.

---

## Architecture: Medallion Pipeline

The lakehouse pipeline processes complaint data across three distinct tiers:

```text
       Raw Data (CFPB API — JSON/CSV)
                 │
                 ▼
┌───────────────────────────────────┐
│     Bronze Layer (Delta Lake)     │  <-- Raw Ingestion + Audit Metadata
│  - Ingestion Timestamp            │      (ingestion_timestamp, source_batch_id)
│  - Batch Tracking (full/incr.)    │
└─────────────────┬─────────────────┘
                  │
                  ▼
┌───────────────────────────────────┐
│     Silver Layer (Delta Lake)     │  <-- Cleansed & Standardized
│  - Date Parsing & Type Casting    │  - Duplicate Complaint Removal
│  - Standardized Categorical       │  - Null / Malformed Record Handling
│    Fields (product, issue, etc.)  │
└─────────────────┬─────────────────┘
                  │
                  ▼
┌───────────────────────────────────┐
│      Gold Layer (Star Schema)     │  <-- Analytics & Business Intelligence
│  - fact_complaint                 │  - dim_company, dim_product, dim_date
│  - agg_complaints_by_company      │  - agg_response_time_by_issue
└─────────────────┬─────────────────┘
                  │
                  ▼
         Power BI Dashboards
```

---

## Repository Structure

```
├── README.md
├── .gitignore
├── data/
│   └── samples/
│       ├── full_load_sample.csv
│       └── incremental_load_sample.csv
└── Notebooks/
    └── Complaint-Analytics-Pipeline
```

## Data Fields

Fields used throughout this pipeline come directly from the CFPB's public schema — no invented or simulated columns:

`complaint_id`, `date_received`, `date_sent_to_company`, `product`, `sub_product`, `issue`, `sub_issue`, `company`, `state`, `zip_code`, `submitted_via`, `company_response`, `timely`, `tags`

## Security & Privacy

The CFPB dataset does not include customer names, account numbers, or transaction amounts. It contains only complaint metadata (company, product, state, dates, response status), so no hashing or masking of direct personal identifiers is required. This is documented in full in the Phase 1 proposal.

## Project Status

This repository currently reflects **Phase 1**: real sample data (full load + incremental load), high-level Medallion architecture design, and project setup. Pipeline notebooks and dashboard implementation follow in later phases.