---
layout: default
title: Amazon OpenSearch
parent: Analytics
grand_parent: AWS Services
math: katex
last_modified_date: 2024-11-15
---

# Amazon OpenSearch
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

Amazon OpenSearch Service unlocks petabyte-scale real-time search, monitoring, and analysis of business and operational data for use cases like application monitoring, log analytics, observability, and website search. It can be managed or serverless.

AWS manages software installing, upgrading, patching, scaling (up to 3 PB), and cross-Region replicating with no downtime. OpenSearch Service is also bundled with a dashboard visualization tool, Amazon OpenSearch Dashboards, which helps visualize not only log and trace data, but also machine learning (ML)-powered results for anomaly detection and search relevance ranking.

[Kinesis Data Streams](docs/analytics/kinesis.html) is a good option for loading data into OpenSearch.

Supports snapshot creation in [Amazon S3](docs/storage/s3.html)

## Concepts

While an SQL database has rows of data stored in tables, OpenSearch stores data as multiple JSON ***documents*** inside an ***index***.

Each document will have a unique id and the index it is stored, both attached as a metadata.

Each *index* is split into **shards** and *shards* may be located into different **nodes** in the **cluster**.

***Index State Management*** automates index management policies. Some examples are: deleting old indices, moving indices to read-only state, moving indices between storage types, change number of replicas, index rollups (summarize data to save cost), index transforms such as grouping and aggregations, and automating index snapshots. They are run every 30 to 48 minutes to ensure not all rules are started at the same time.

## Redundancy

Each index will be composed of ***Primary Shards*** and ***Replica Shards*** (optional). *Primary shards* are the entry point for *write* requests and are then responsible for replicating data to the replica shards. Both the *Primary Shards* and the *Replica Shards* can be used for *read* operations, being responsability of the consumer application to round-robin between them for maximum benefit.

OpenSearch can do **cross-cluster replication** for high availability or better latency. *Remote Reindex* is the same, but for a single index.

## Storage

There are three storage options and the data may be migrated between different storage types:

- Stardard **Hot** storage: Uses instance store or EBS for fastest performance.
- **UltraWarm** (sometimes called Warm): uses S3 and a caching layer and must have a dedicated master node. Useful for indices with few writes (immutable data/logs). Slower performance but better cost.
- **Cold**: uses S3 but there is no caching layer. The cheapest of the options, with the slowest performance. Must have a dedicated master node and UltraWarm enabled. Ideal for periodic research.

## Security

Security features/compatibility include:

- Resource-based policies
- Identity-based policies
- Ip-based policies
- VPC (has to be decided before cluster creation)
- Cognito (easiest way to access Dashboards)
- Request Signing

## Anti-patterns

- Transactional Data: use [RDS](docs/database/rds.html) or [DynamoDB](docs/database/dynamodb.html) instead;
- Ad-hoc Data Querying: use [Amazon Athena](docs/analytics/athena.html) instead.
