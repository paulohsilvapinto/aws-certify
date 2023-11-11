---
layout: default
title: DynamoDB
parent: Databases
grand_parent: AWS Services
math: katex
---

# DynamoDB
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

- NoSQL database. It is serverless, fully managed, and replicated across multi-AZs. It can handle a vast amount of data due to its distributed nature. It is a low-cost service and can adapt well to the workload with its auto-scaling capabilities.
- No support for query joins and aggregations.
- The Primary Key must be created at the table creation time. The same goes for the Sort Key; however, unlike the Primary Key, a Sort Key is not mandatory.
- Each row can have a maximum of **400KB**, and the attributes (fields) can store nested data like list, map, and set, just like a JSON record.

## Use cases

- DynamoDB is recommended for a variety of scenarios such as Mobile apps, IoT, Gaming, etc,
- **Anti-patterns** include: using for data that requires complex transactional data and SQL instructions; modifying from a relational database to a NoSQL database if the application was prepared to work with a relational DB. 
- **Large objects** cannot be stored directly on DynamoDb due to the limitation of file size, but a typical pattern is storing the data in S3 and the metadata in the DynamoDB table.

{: .note }
> In general: 
> - Large volume of data with multiple reads and write requests: DynamoDB;
> - Large volume of data, but data is not retrieved/modified frequently: S3;
> - Transactional data with complex SQL instructions: RDS.

## Primary Keys

There are two options for the primary keys, and you must choose at the table creation time:

### Partition Key (HASH Key)

- The key must uniquely identify each record, and it is used to distribute the data across the different DynamoDB partitions. It's better to have a random partition key to avoid data skewness.


### Partition Key + Sort Ket (HASH + RANGE Keys)

- The combination of both the HASH and the RANGE keys must uniquely identify each record.
- The partition key will have the same functionality as before, and the Sort key will be used to sort the records inside the partition.
- This option is best suited for scenarios where you want to store related records in the same place (same Node) so that data retrieval is faster. For example, user_id + order_id.

## Storage Tiers

Per table, can be:

- Standard
- Infrequent Access

## Capacity Modes

Table capacity is defined by the RCU (Read Capacity Units) and WCU (Write Capacity Units). There are two modes for setting these variables, and it's possible to change between them once every 24 hours. They can be:

- **Provisioned**: RCU and WCU must be supplied according to the expected workload and you can optionally enable the auto-scale. They can be exceeded momentarily. In this case, the extra consumption will come from the **Burst Capacity**. If the Burst Capacity is depleted, a *ProvisionedThroughputExceededException* will be raised. Exponential backoff is recommended to normalize the operation.
- **On-demand**: Automatically scales RCU and WCU up or down, but is more expensive than Provisioned mode.

### WCU (Write Capacity Units)

- WCU represents one item per second for an item of up to 1 KB in size (rounded up).

{: .note }
> $$
> WCU = ItemsPerSecond * \frac{ItemSizeKb}{1Kb}
> $$