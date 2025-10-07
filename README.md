# Databricks + AWS Stock Data Lakehouse  
### Building a 3-Tier S3 Data Lake with PySpark, Glue, Athena, and QuickSight  

**Portfolio project demonstrating a cloud data lakehouse architecture on AWS with Databricks and PySpark.**  
This project implements a 3-layer (raw, processed, curated) data lake design for scalable and queryable stock analytics.  
Data is ingested, transformed, and stored as partitioned Parquet files on S3, integrated with AWS Glue, Athena, Redshift, and QuickSight for metadata management and visualization.  

🔹 **Technologies:** Databricks · PySpark · AWS S3 · Glue · Athena · Redshift · QuickSight  
🔹 **Focus:** Cloud data engineering architecture, PySpark transformation, and end-to-end analytics pipeline  
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
│   └── AWS Databricks PySpark Stock Data Lakehouse.ipynb
│  
├── screenshots/
│   ├── aws_s3
│   ├── aws_glue
│   ├── aws_redshift
│   ├── aws_athena
│   └── aws_quicksight
│  
├── README.md
└── LICENSE
```
