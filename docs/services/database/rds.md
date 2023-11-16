---
layout: default
title: RDS - Relational Database Service
parent: Databases
grand_parent: AWS Services
---

# RDS - Relational Database Service
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

RDS is a hosted, [ACID-compliant][1], relational database that may be used mainly for storing transactional data. You can choose Amazon Aurora, MySQL, PostgreSQL, Oracle, etc. Cloudwatch can be used to monitor the RDS instance's health.

For better security, it is recommended to use an API Gateway as an entry point and use its rate limits to avoid DDoS attacks. It is also recommended that any application accessing the database should have a small TTL (Time to Live) on RDS's DNS, so that in case a failover is needed, the application does not break due to the cached DNS. 

Some benefits of RDS over an on-premises database are:

- Auto-scaling;
- Patches and Updates applied automatically;
- Continuous and automatic backups; etc.

## Amazon Aurora

It is a hosted and faster alternative to either MySQL or PostgreSQL. It can store up to 128TB per volume and allows up to 15 read replicas (**eventually consistent reads**) for faster data access. Regarding availability, it offers multi-region/multi-AZ replication and continuous backup to S3. Data is encrypted at rest with KMS, in-transit with SSL, and provides VPC isolation.

### Aurora Serverless

Another flavour of Amazon Aurora that offers a fully-managed RDS with automatic scaling capabilities. Ideal for scenarios where the workload fluctuates frequently.

## RDS on Outposts / RDS on VMWare

Allow the creation of hybrid or on-premises RDS instances. Supported only on MySQL and PostgreSQL. Not supported on Aurora.

## ACID

Atomic
: If part of the transaction fails, then the whole transaction fails.

Consistent
: Ensures constraints are respected.

Isolated
: One transaction does not impact another. Important for concurrency.

Durable
: Any change to the database are persisted when the transaction succeeds.


[1]: #acid