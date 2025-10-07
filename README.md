# databricks-pyspark-aws-pipeline
Portfolio project demonstrating a cloud data lakehouse architecture on AWS with Databricks and PySpark. Includes raw, processed, and curated layers for scalable analytics.
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
                 
```
databricks-aws-stock-lakehouse/
├── notebooks/
│   └── stock_etl_databricks.ipynb
├── assets/
│   ├── architecture.png
│   ├── quicksight_dashboard.png
│   ├── glue_catalog.png
│   └── athena_query.png
├── scripts/
│   └── pyspark_etl.py
├── README.md
└── LICENSE
```
