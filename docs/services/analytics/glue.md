---
layout: default
title: AWS Glue
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-06
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

Data Catalog is a centralized metadata catalogue that allows data to be queried from [Amazon Athena](docs/analytics/athena.html), [Amazon EMR](docs/analytics/emr.html) (with SQL-like queries with *Hive*), [Amazon Redshift Spectrum](docs/analytics/redshift.html) and [Quicksight](docs/analytics/quicksight.html, and it is important in a *Data Lake* to allow unstructured data to be queried.

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
- [Redshift](docs/analytics/redshift.html)
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

## Costs

Generally speaking, you pay for what you use.

For development endpoints, however, AWS charges by the minute whenever the endpoint is up. That said, be sure to kill the endpoint whenever it is not in use.
