# databricks-pyspark-aws-pipeline
Portfolio project demonstrating a cloud data lakehouse architecture on AWS with Databricks and PySpark. Includes raw, processed, and curated layers for scalable analytics.
          yfinance API
               │
               ▼
        ┌──────────────┐
        │   Raw (S3)   │  ← 原始 CSV
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
