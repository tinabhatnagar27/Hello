# Apache Iceberg

## Overview

Apache Iceberg is an **open table format** for large-scale analytics. It helps you manage huge datasets stored in data lakes (S3, GCS, Azure Blob, HDFS) in a **safe, fast, and reliable** way.

Think of Iceberg as a **smart layer** on top of raw data files (Parquet, ORC, Avro) that makes them behave like a proper database table with:

* ACID transactions
* SQL support
* Version control (time travel)
* Schema & partition evolution
---

## Why Apache Iceberg Exists

Traditional data lakes:

* Don’t support ACID transactions 
* Can break if files are deleted accidentally 
* Are slow for analytics 

Apache Iceberg solves this by:

* Tracking metadata safely
* Preventing partial reads/writes
* Allowing rollback to old data versions

---

## What Is Apache Iceberg?

Apache Iceberg is **not a database**.
It is a **table format** that organizes data files so analytics engines see them as **one logical table**.

 Files on storage → Iceberg metadata → Query engine

---

## Short History

* **2017** – Created by Netflix to fix Hive limitations
* **2018** – Donated to Apache Foundation
* **2020–2023** – Massive industry adoption
* **2024** – Databricks plans to unify Iceberg & Delta Lake

---

## What Is Apache Iceberg Used For?

### 1. Fast Analytics

Iceberg stores **metadata** about files, so engines read **only required data**, not everything.

### 2️. Partition Pruning (Performance Boost)

Only reads partitions matching query filters (e.g., date range).

### 3️. Time Travel

Query old data versions:

```sql
SELECT * FROM table VERSION AS OF 10;
```

Useful for:

* Debugging
* Audits
* Accidental deletes recovery

### 4️. Multi-Engine Support

Works with:

* Spark
* Flink
* Trino
* Presto
* Hive

---

## Core Concepts 

### Metadata

Iceberg tracks everything using metadata files:

* Table schema
* Partition info
* File locations
* Snapshot history

### Snapshots

Every change creates a **new snapshot**.
Snapshots = full table state at a time.

### Schema Evolution

Change schema **without rewriting data**:

* Add column
* Rename column
* Drop column

### Partition Evolution

Change partition strategy anytime (no data rewrite).

Partition types:

* Range
* Hash
* Truncate
* List

---

## Apache Iceberg Architecture (Simple)

### 1️. Catalog Layer

Stores pointer to latest metadata file
Examples:

* Hive Metastore
* AWS Glue

Ensures ACID transactions 

### 2️. Metadata Layer

Contains:

* Metadata files (schema, snapshots)
* Manifest files (file-level info)
* Manifest lists (group of manifests)

### 3️. Data Layer

Actual data stored as:

* Parquet
* ORC
* Avro

---

## Integrations

### Apache Spark

* Read & write using DataFrames
* Query using Spark SQL

### Apache Flink

* Best for streaming ingestion

### Trino / Presto

* Ultra-fast SQL analytics
* Uses external catalogs

---

## Cloud Compatibility

| Cloud      | Usage                      |
| ---------- | -------------------------- |
| AWS S3     | Glue Catalog + Spark/Trino |
| GCS        | BigQuery / Spark           |
| Azure Blob | Synapse / Spark            |

---

## Apache Iceberg vs Delta Lake

| Feature             | Iceberg            | Delta Lake         |
| ------------------- | ------------------ | ------------------ |
| Engine support      | Multi-engine       | Spark-focused      |
| File formats        | Parquet, ORC, Avro | Parquet only       |
| Partition evolution | Yes                | Limited            |
| Vendor lock-in      | No                 | Databricks-centric |

 **Use Iceberg** if you use multiple engines
 **Use Delta** if you are Spark-only

---

## Getting Started (Local Setup)

### Prerequisites

* Java JDK
* Python
* PySpark
* Apache Spark

---

### Create Warehouse

```bash
mkdir iceberg-warehouse
```

---

### Start Spark with Iceberg

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("Iceberg Demo") \
    .config("spark.sql.catalog.my_catalog", "org.apache.iceberg.spark.SparkCatalog") \
    .config("spark.sql.catalog.my_catalog.type", "hadoop") \
    .config("spark.sql.catalog.my_catalog.warehouse", "file:///path/iceberg-warehouse") \
    .getOrCreate()
```

---

## Basic Operations

### Create Table

```sql
CREATE TABLE my_catalog.emp (
  id INT,
  name STRING
) USING iceberg;
```

### Insert Data

```sql
INSERT INTO my_catalog.emp VALUES (1, 'James'), (2, 'John');
```

### Delete Data

```sql
DELETE FROM my_catalog.emp WHERE id = 1;
```

---

## Advanced Techniques

### Schema Evolution

```sql
ALTER TABLE my_catalog.emp ADD COLUMN salary INT;
```

### Partition Evolution

```sql
ALTER TABLE my_catalog.emp ADD PARTITION FIELD days(join_date);
```

### Time Travel

```sql
SELECT * FROM my_catalog.emp VERSION AS OF 5;
```

---

## Best Practices

* Use **hidden partitions**
* Compact small files regularly
* Monitor metadata size
* Prefer Iceberg for multi-engine setups

---

## Conclusion

Apache Iceberg brings **database reliability** to data lakes.
It is:

* Safe
* Scalable
* Cloud-friendly
* Future-proof

Perfect for modern **lakehouse architectures** 

---
