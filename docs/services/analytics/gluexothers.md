---
layout: default
title: AWS Glue x Others
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-07
---

# AWS Glue x Others
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

AWS Glue compared to other AWS Services.

## Glue versus Kinesis Data Analytics

### AWS Glue

- Spark-based;
- ETL is its primary use case;
- Ideal for transforming/loading data into the Data Lake.

### Kinesis Data Analytics

- Flink-based;
- Primary use case is Analytics;
- Ideal for real-time analytics.

## Glue versus Kinesis Data Firehose

### AWS Glue

- Complex ETLs with possibility to join different streaming data.
- Has more Targets available than Kinesis Data Firehose.

### Kinesis Data Firehose

- Simple ETL with the help of Lambda Functions and simple conversion from JSON to Parquet.
- Target is limited to Splunk, [S3](docs/storage/s3.html), [Redshift](docs/database/redshift.html) and Elastic Search.


## Glue versus Amazon EMR

### AWS Glue

Jobs Spark or Python

### Amazon EMR

Jobs on a Hadoop environment, supporting any framework.
