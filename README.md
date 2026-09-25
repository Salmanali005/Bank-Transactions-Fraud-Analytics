# Bank Transaction Analytics — Fraud & Spend Insights Lakehouse

An end-to-end data engineering project built on Apache Spark, following the Medallion (Bronze–Silver–Gold) Architecture, to analyze bank transaction data for spend behavior and fraud pattern insights.

## Team

- Salman Ali
- Ahmad Khan

**Course:** Data Analysis and Visualization — Semester 5

## Project Overview

This project simulates the data engineering workflow of a retail bank's transaction monitoring and analytics team. Raw transaction data is ingested, cleansed, and modeled through a Lakehouse pipeline to produce business-ready tables for spend analysis and fraud rate monitoring.

**Domain:** Banking / Fintech
**Platform:** Databricks Community Edition (Apache Spark)
**Dashboarding:** Power BI

## Data Source

We use the **PaySim** synthetic mobile-money transaction dataset (Kaggle), which mirrors the structure of real banking transaction data without exposing any actual customer information.

- **Full Load:** A large historical batch of transactions, ingested once to seed the Bronze layer.
- **Incremental Load:** Smaller batches of new transactions, simulating periodic (daily) data arrival from a live banking system.

Sample data files for both load types are available in `data/samples/`.

## Repository Structure

```
├── README.md
├── docs/
│   └── Phase1_Project_Proposal.docx
├── data/
│   └── samples/
│       ├── full_load_sample.csv
│       └── incremental_load_sample.csv
└── Fraud-Analytics
```

## Architecture

The pipeline follows the Medallion Architecture:

- **Bronze:** Raw transaction data ingested as-is, with metadata added for traceability.
- **Silver:** Cleansed and standardized data — type casting, deduplication, and PII handling (hashing of account identifiers).
- **Gold:** Business-ready dimensional model (fact and dimension tables) supporting spend and fraud analytics.

Full details are documented in `docs/Phase1_Project_Proposal.docx`.

## Security & Compliance

Account identifiers and other sensitive fields are hashed or masked before reaching the Silver layer. No raw personally identifiable information is retained beyond the Bronze layer. See the proposal document for the complete handling strategy.

## Project Status

This repository currently reflects **Phase 1** of the project: domain and data source definition, sample data, and high-level architecture planning. Pipeline notebooks and dashboard implementation will follow in later phases.