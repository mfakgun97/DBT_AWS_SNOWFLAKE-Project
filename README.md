# DBT + Snowflake + AWS S3 — Airbnb Data Pipeline

## 📋 Project Overview

This project builds an end-to-end data pipeline for Airbnb data (listings, bookings, hosts). Raw data is securely stored in AWS S3, loaded into Snowflake, and then cleaned and transformed using dbt (data build tool) following a **medallion architecture** (Bronze → Silver → Gold).

## ⚙️ Tech Stack

- **AWS S3** — data lake storing raw CSV files (bookings, listings, hosts). Access is secured through **IAM (Identity and Access Management)** policies, ensuring only authorized, scoped access to the bucket.
- **Snowflake** — cloud data warehouse. Raw data is loaded from S3 into the `STAGING` schema using `COPY INTO` commands.
- **dbt (data build tool)** — manages SQL-based transformation layers on top of Snowflake:
  - **Bronze** — ingests raw source data using an incremental loading strategy
  - **Silver** — applies data cleaning, type casting, and business logic (categorical tagging, formatting)
  - **Gold** — produces joined/aggregated tables for analytics and reporting (One Big Table & star schema approach)
- **Snapshots** — track how data changes over time (SCD Type 2).
- **Custom Macros** — encapsulate reusable transformation logic (e.g., categorical tagging) to keep models DRY.
- **Metadata-driven modeling** — parts of the join logic are dynamically generated via Jinja using config lists, reducing repeated code across models.

## 🔐 Security

- AWS access is scoped and restricted via IAM policies — credentials are never committed to the repository.
- Snowflake connection details are managed through `profiles copy.yml` and excluded from version control via `.gitignore`.

## 🚀 Usage

```bash
dbt debug          # Test the Snowflake connection
dbt run            # Run all models
dbt test           # Run data quality tests
dbt snapshot       # Run change-tracking snapshots
```
