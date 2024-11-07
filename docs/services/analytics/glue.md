---
layout: default
title: AWS Glue
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-07
---

# AWS Glue
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

AWS Glue is a serverless data integration service used for data discovery, preparation, cleansing, transformation, and integration from multiple sources.

As it is based on Spark, an anti-pattern would be using for projects requiring multiple ETL engines such as Hive, Hadoop, etc; in which case [Amazon EMR](docs/analytics/emr.html) would be better.

{: .important }
Using Glue for processing Streaming data **used** to be an anti-patter, but it is no longer the case, as it can consume [Kinesis](docs/analytics/kinesis.html) or Apacha Kafka thanks to Apache Spark Structured Streaming.

## Glue Components

### Data Catalog

Data Catalog is a centralized metadata catalogue that allows data to be queried from [Amazon Athena](docs/analytics/athena.html), [Amazon EMR](docs/analytics/emr.html) (with SQL-like queries with *Hive*), [Amazon Redshift Spectrum](docs/database/redshift.html) and [Quicksight](docs/analytics/quicksight.html), and it is important in a *Data Lake* to allow unstructured data to be queried.

The Data Catalog contains information such as:

- Database Name
- Table Name
- Column Names
- Data Types
- Data Partitioning  - Based on how data is stored in S3, can cause big impact if not defined well. For example, suppose you receive files from different partners every day. If you are most likely explore data per partner, partitioning the data as `partner=A/year=X/month=Y/day=Z` will perform better than partitioning as `year=X/month=Y/day=Z/partner=A`

The data itself is not part of the Data Catalog.

The Glue Data Catalog is compatible with query in *Hive* and vice-versa.

### Crawlers

Crawlers are processes used for scanning structured and unstructured data and then uses the gathered information to populate or update the Data Catalog.

Sources can be:

- [S3](docs/storage/s3.html)
- [RDS](docs/storage/rds.html)
- [Redshift](docs/database/redshift.html)
- [DynamoDB](docs/storage/dynamodb.html)
- *JDBC*: Java Database Connectivity
- [Glue Data Catalog](#data-catalog)

Crawlers can be scheduled, triggered on-demand or triggered as part of a Glue Workflow.

It is possible to create custom classifiers for crawling custom data types.

### Glue Jobs

A fully managed process that is used for ETL. Can be a pure Python, Scala or a PySpark script. Can be event-driven and can do ETL on streaming data by using continuously-running jobs. In some circumstances, it is possible to update the catalog directly via the Glue Job, without requiring a crawler to be re-executed.

**DPUs**, or *Data Processing Units*, can be increased to increase the compute capability of each node. In addition, it is possible to add more nodes to job and it can be scaled on-demand.

Has at rest and in transit data encryption.

Jobs can be scheduled or triggered by *Glue Triggers*.

#### Job Bookmark

Glue Jobs have an optional feature called Bookmarks. This basically persists the state of the job, preventing the same data getting processed again and allowing you to start the next execution exactly where you ended the last one.

In relational databases, job bookmarks can only keep track of new rows. It cannot keep track of updates.

### Workflows

A Glue-only orchestrating component. If any other integration is required, such as a Lambda Function, for example, then Glue Workflow cannot be used.

The workflows can be triggered by schedule, on demand or by EventBridge events with optional batching available

{: note }
EventBridge is the only *non-Glue* component that offers integration with AWS Glue workflows.

### Glue Studio

Glue Studio is a visual interface for building complex ETL ***DAGs*** (Directed Acyclic Graph, or Workflows).

Sources can be:

- [S3](docs/storage/s3.html)
- [RDS](docs/storage/rds.html)
- [Redshift](docs/database/redshift.html)
- [Kinesis](docs/storage/dynamodb.html)
- [Kafka](docs/analytics/kafka.html)

And possible targets are:

- [S3](docs/storage/s3.html) with partitioning support.
- [Glue Data Catalog](#data-catalog)

The studio also provides a dashboard for job overview, status, etc.

### Data Quality

It is possible to add to a job/workflow a Data Quality step for ensuring the data meet some pre-established parameters. In case of failure, you can fail the job or integrate with other services like Cloudwatch to report the unexpected behaviour.

The rules used for the Data Quality check can be defined manually by using the DQDL (*Data Quality Definition Language*) or automatically, where Glue will try to find the patterns on a sample data and create some expected rules.

## Costs

Generally speaking, you pay for what you use.

For development endpoints, however, AWS charges by the minute whenever the endpoint is up. That said, be sure to kill the endpoint whenever it is not in use.
