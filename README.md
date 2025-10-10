# Databricks real-time streaming + AWS Stock Data Lakehouse  
### Building a 3-Tier S3 Data Lake with PySpark, Kinesis(Kafka), Glue, Athena, and QuickSight  

**Portfolio project demonstrating a cloud data lakehouse architecture on AWS with Databricks and PySpark.**  
This project implements a 3-layer (raw, processed, curated) data lake design for scalable and queryable stock analytics.  
Data is ingested, transformed, and stored as partitioned Parquet files on S3, integrated with AWS Glue, Athena, Redshift, and QuickSight for metadata management and visualization.  

🔹 **Technologies:** Databricks · PySpark · AWS S3 · Glue · Athena · Redshift · QuickSight · Distributed Computing · Serverless · Parquet Partitioning
🔹 **Focus:** Cloud data engineering architecture, PySpark transformation, and end-to-end analytics pipeline  

Architecture Overview

Storage: S3 hosts the multi-layer data lake (raw → processed → curated → features).
Compute: Databricks (serverless or cluster mode) runs PySpark for large-scale ETL and feature engineering.
Metadata: AWS Glue crawlers catalog the data for downstream SQL tools.
Query & Analysis: Athena and Redshift provide serverless or warehouse-level querying.
Visualization: QuickSight powers interactive dashboards and analytics.

```
                   yfinance API
                         │
                         ▼
                  ┌──────────────┐
                  │   Raw (S3)   │  ← Original CSV
                  └──────────────┘
                         │
                         ▼
               ┌──────────────────┐
               │ Databricks +     │
               │ PySpark ETL Job  │
               └──────────────────┘
                         │
                         ▼
             ┌───────────────┐
             │ Processed (S3)│ ← Parquet, Features
             └───────────────┘
                         │
                         ▼
             ┌───────────────┐
             │ Curated (S3)  │ ← BI-ready, Delta
             └───────────────┘
                 │         │
                 ▼         ▼
              Glue      Athena/Redshift
              Catalog       │
                 │         ▼
                 └────► Tableau/QuickSight
```

### Project Structure
```
databricks-aws-stock-lakehouse/
├── notebooks/
│   └── AWS Databricks PySpark Stock Data Lakehouse.ipynb   # Main end-to-end notebook
│
├── screenshots/
│   ├── aws_s3/          # S3 multi-layer and partition structure
│   ├── aws_glue/        # Glue crawlers & Data Catalog
│   ├── aws_redshift/    # (Optional) Redshift schema / external tables
│   ├── aws_athena/      # Athena queries and results
│   └── aws_quicksight/  # QuickSight dashboards
│
├── README.md
└── LICENSE
```

### Pipeline Steps

### Step 1. S3 Data Layers (Raw → Processed → Curated → Features)

What:
Organized data in layered S3 folders:
raw/: unprocessed CSVs downloaded from APIs (e.g., yfinance).
processed/: cleaned and type-casted data written as Parquet.
curated/: structured, query-ready Parquet with partitions (ticker, year).
curated/stocks_features/: enriched feature layer — SMA, RSI, MACD, Bollinger Bands, volume MAs, buy/sell signals, golden/death crosses.

Why:

Enables separation of concerns — each layer has a distinct purpose.
Columnar + partitioned Parquet improves query speed and cost.
Future-proof — new metrics can be appended without rewriting raw data.

![aws_s3](screenshots/aws_s3/s3_layers.png)
![aws_s3](screenshots/aws_s3/s3_ticker.png)

### Step 2. Compute Layer: Databricks + PySpark

What:

Implemented ETL and feature engineering inside
notebooks/AWS Databricks PySpark Stock Data Lakehouse.ipynb.

Performed cleaning, casting, and window-based technical indicators:
SMA20/50/200, RSI14, MACD(12,26,9), Bollinger Bands, volume MAs, buy/sell flags.

![databrick](screenshots/databrick/databrick_example.png)


Why:

PySpark provides distributed big-data processing across clusters.
Window functions are natively optimized and scalable.
Databricks Serverless eliminates infrastructure management — compute on demand, pay per use.

### Step 3. Metadata Layer: AWS Glue

What:

Created Glue Crawlers to scan S3 folders (processed/, curated/) and register metadata in the Glue Data Catalog.

Why:

Centralized metadata shared across AWS services (Athena, Redshift, EMR).
Automatically detects partitions and schema evolution.
Supports data governance and lineage tracking.

![aws_s3](screenshots/aws_glue/glue_schema.png)

### Step 4. Serverless Querying: Athena

What:

Queried Glue-registered Parquet tables directly using Athena SQL.
Example query:

Why:

Fully serverless SQL engine — no cluster setup, pay only per data scanned.
Works efficiently with Parquet + partitions, minimizing scan cost.

![aws_s3](screenshots/aws_athena/athena_query.png)

### Step 5. Data Warehouse Layer: Redshift / Spectrum

What:

Two integration options:
Redshift Spectrum — query external Parquet data via Glue catalog.
Native Redshift Tables — load curated data for faster joins and aggregations.

Why:

Redshift provides high-performance OLAP for heavy BI workloads.
Hybrid model: keep hot data in Redshift, cold data in S3 (cost-efficient).

![aws_s3](screenshots/aws_redshift/redshift_overview.png)

### Step 6. Visualization Layer: QuickSight

What:

Connected to Athena/Redshift datasets.
Built visual dashboards — line charts, RSI thresholds (30/70), MACD histograms, Bollinger bands, and comparative performance.

Why:

Cloud-native BI, zero maintenance, and SPICE acceleration.
Easy sharing and IAM-based access control.

![aws_s3](screenshots/aws_quicksight/quicksight_avg_price.png)

### Schema & Partitioning Design

Storage Format: Parquet (columnar, compressed).
Partitions: ticker, year for time-series optimization.
Schema Evolution: Processed layer defines canonical schema; feature layer appends new fields safely.

### Security & Permissions

Used IAM Roles / Instance Profiles to grant Databricks access to S3, Glue, and Athena.
Followed least privilege principle (path-level access).
Optional VPC endpoint for secure private network communication.

### Near Production-Grade

Multi-layered S3 data lake architecture.
Scalable distributed PySpark computation.
Centralized metadata (Glue Catalog).
Serverless query layer (Athena / optional Redshift).
Integrated BI visualization (QuickSight).
Clear folder hierarchy + reproducible notebooks.

### Screenshot Guide
Folder	Recommended Content
aws_s3/	Three-tier S3 folder layout showing partitions (ticker= / year=).
aws_glue/	Glue crawler setup, databases, and partition discovery pages.
aws_athena/	SQL queries and results (showing low data scan cost).
aws_redshift/	External schema or warehouse table queries.
aws_quicksight/	Dashboard overview and key charts (trend, RSI, MACD, Bollinger Bands).


