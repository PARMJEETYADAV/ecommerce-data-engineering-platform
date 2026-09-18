# Ecommerce-data-engineering-platform

## Overview

The **ecommerce-data-engineering-platform** is a comprehensive data engineering solution for e‑commerce analytics. It provides a full end‑to‑end pipeline that ingests raw Olist CSV datasets, transforms them into a star‑schema data warehouse in PostgreSQL, runs data‑quality checks, and surfaces the results through Apache Superset dashboards.

The platform demonstrates best‑practice data‑engineering patterns such as:

- **Medallion Architecture** – Bronze (raw staging), Silver (cleaned staging), Gold (dimensional model).
- **Orchestration** – Apache Airflow DAGs coordinate all ELT steps.
- **Containerisation** – Docker Compose defines services for Airflow, PostgreSQL, Kafka, Zookeeper and Superset for reproducible environments.
- **Data Quality** – Automated checks for row counts, null values, referential integrity and domain constraints.
- **Observability** – Built‑in logging and Airflow UI for pipeline monitoring.

## Repository Structure

```
.
├── dags/                     # Airflow DAG definitions
│   ├── ingest_olist_staging.py   # Ingest raw CSVs into staging tables
│   ├── ingest_orders_csv.py       # Load orders CSV into staging
│   ├── build_olist_dw.py          # Build the data‑warehouse (dim & fact tables)
│   └── dq_olist_dw.py            # Data‑quality checks for the DW
├── dashboards/superset/      # Superset dashboard YAML definitions
├── data/                     # **Ignored** – place raw CSVs here (data/raw/)
├── docker-compose.yaml       # Docker services configuration
├── Dockerfile                # Base image for Airflow workers
├── requirements.txt          # Python dependencies
├── sql/                      # SQL scripts for DW creation & views
└── README.md                 # **This file**
```

## Quick Start (Local Execution)

1. **Prerequisites**
   - Python 3.11+ (or use the provided Docker environment).
   - Git installed and authenticated with your GitHub account.
   - (Optional) Docker Desktop if you prefer containerised execution.

2. **Clone the repository**
   ```bash
   git clone https://github.com/PARMJEETYADAV/ecommerce-data-engineering-platform.git
   cd ecommerce-data-engineering-platform
   ```

3. **Install Python dependencies**
   ```bash
   python -m venv venv
   source venv/Scripts/activate   # PowerShell: venv\Scripts\Activate.ps1
   pip install -r requirements.txt
   ```

4. **Prepare raw data**
   - Download the Olist dataset (8 CSV files) from the public source.
   - Copy the files into `data/raw/` (the folder is listed in `.gitignore`).

5. **Run the Airflow pipeline locally**
   ```bash
   airflow db init
   airflow users create \
       --username admin \
       --firstname admin \
       --lastname user \
       --role Admin \
       --email admin@example.com
   airflow scheduler &
   airflow webserver
   ```
   - Access the Airflow UI at `http://localhost:8080` and trigger the DAGs.

6. **Or use Docker Compose** (recommended for full stack)
   ```bash
   docker compose up -d
   ```
   - Airflow UI: `http://localhost:8080`
   - Superset UI: `http://localhost:8088` (login: admin / admin)

## Author

This repository is maintained by **PARMJEETYADAV** (GitHub user `PARMJEETYADAV`). All Airflow DAGs now reference this owner name in the `default_args` configuration.

## License

Open‑source under the MIT License. See `LICENSE` for details.

---
*Feel free to open issues or submit pull requests for enhancements, bug fixes, or additional documentation.*
