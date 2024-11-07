---
layout: default
title: Amazon Athena
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-07
---

# Amazon Athena
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## General

Amazon Athena is a serverless interactive query service, built on top of **Presto**, for accessing any data (relational, non-relational, structured, semi-structured or unstructured) directly from [S3](docs/storage/s3.html) (in different S3-tier levels) via [Glue Catalog](docs/analytics/glue.html#data-catalog), on-premises or from multicloud environment.

Supports many data formats, such as: CSV and JSON as human readable formats; ORC and Parquet as columnar and splittable (parallelization) formats and Avro a non-columnar but splittable format.

It is compatible with QuickSight and other tools via ODBC/JDBC and it is possible to do cross-account data access.

## Athena Workgroups

It is a feature for better management of Athena Users/Teams/Applications, as it can control query access, track costs, set IAM policies, Data limits, Encryption settings and keep a query history per Workgroup.

Workgroups integrate with IAM, SNS and CloudWatch.

## ACID Transactions

Thanks to *Apacha Iceberg*, *Athena* supports transactional tables with time travel features. To create a table using Iceberg, just add `'table_type' = 'ICEBERG'`.

These tables are compatible with EMR, Spark, Glue, etc.

With time these tables may get slower, so it is recommended to perform a periodic compaction. To do that, run the command:

```SQL
OPTIMIZE table_name REWRITE DATA
  USING BIN_PACK
  WHERE catalog = 'catalog_name';
```

## CTAS - Create Table as Select

Used to create a table from the result of a SQL Query. Can be useful for creating a Backup table, a smaller table with a subset of data or even for converting the file data type.

For example:

```SQL
CREATE TABLE new_table_name
WITH (
    format='Parquet',
    write_compression='SNAPPY',
    external_location='s3://path/to/file'
)
AS SELECT *
FROM old_table_name;
```

## Fine-Grained Access Control

The access control in Athena is not as restrictive as if you were using *AWS Lake Formation*. For example, it is not possible to restrict column/row access.

It is possible to allow or disallow some operations DDL/DB Management instructions though, such as `ALTER` or `CREATE`, `DROP`, `MSCK REPAIR TABLE` etc. However, it is not straight forward, as you will need to block/allow some/all the IAM Actions required for the particular operation.

## Cost and Performance

It is a pay-as-you-go service, charged based on bytes of Data. This means failed queries or DDL instructions are free, but successful or cancelled queries are not.

The best way of saving on cost and increasing performance is compressing the data (with GZIP, for example), using columnar and splittable file formats, such as ORC and Parquet, partitioning the data and using a smaller number of large files instead of a large number of small files.

## Anti-patterns

Athena is not recommended for:

- Highly formatted reports;
- ETL.

## Amazon Athena for Apache Spark

It is selectable as an alternate analytics engine and allows Jupyter notebooks with Spark within the Athena Console.

It is serverless and is priced based on compute usage and DPU per hour.
