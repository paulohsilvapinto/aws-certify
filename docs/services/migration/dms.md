---
layout: default
title: Database Migration Service
parent: Migration
grand_parent: AWS Services
last_modified_date: 2024-11-13
---

# AWS Database Migration Service (DMS)
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

As its name suggests, it is a Database Migration Service. It can be from anywhere to anywhere. It is safe, self-healing, fast and resilient.

Provides continuous replication via CDC tables (*Change Data Capture*), without affecting the source Database, which remains available. Requires an *EC2 instance* for the replication processes.

Supports **homogeneous** (from/to the same DB Engine) migrations. It also supports **heterogeneous** (from/to different DB Engines), but it requires an additional tool called ***Schema Conversion Tool (SCT)*** for translating the schema. For best practices, the SCT should be installed in the source database.

An alternative to using *SCT* is using ***Basic Schema Copy**, but in this case secondary indexes, foreign keys or stored procedures will not be migrated.

It has multi-AZ support, with one primary instance and another standby instance that are kept synchronized synchronously. Good for redundancy and minimized latency.

Some supported sources are: On-premises and EC2 instances databases, RDS, S3, DocumentDB, etc.

Some supported target are:  On-premises and EC2 instances databases, RDS, S3, DocumentDB, Redshift, Kinesis Data Streams, DynamoDB, Neptune, etc.
