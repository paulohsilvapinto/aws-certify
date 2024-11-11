---
layout: default
title: Amazon Kinesis
parent: Analytics
grand_parent: AWS Services
math: katex
last_modified_date: 2024-11-11
---

# Amazon Kinesis
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

A managed alternative to *Apache Kafka*. The data is automatically replicated in 3 *AZs* and is composed of three components:

- ***Kinesis Data Streams***
- ***Kinesis Data Firehose***
- ***Kinesis Data Analytics:*** **Deprecation scheduled for 2026.**

## Kinesis Data Streams

A service for streaming *Big-data* data into your systems. ***Producers*** ingest data into the Stream while ***Consumers*** read data from the Stream. Each *Stream* can handle multiple *Producers* and multiple *Consumers*.

### Shards

A *Stream* is divided into multiple ***shards*** (like partitions). The number of *shards* defines the stream capacity of ingestion and consumption.

There is no sorting across shards. The data is sorted only within each shard, based on the partition key of the record.

There are two **capacity modes** for creating a stream:

On **Provisioned mode**:

- The number of *shards* must be chosen in advance, but it can scale later manually or via API;
- **Each *shard* can receive 1MB/s or 1.000 messages/s of data**
- **Each *shard* can serve 2 MB/s of data for the consumers**. If **Enhanced Fan-Out is enabled, than it can serve 2MB/s per shard per consumer**.;
- The cost is defined by the number of shards per hour.
- Each *shard* can handle a **maximum of 5 Read Transactions/second/shard**.

On **On-demand mode**

- The Stream scales automatically based on the throughput peak in the last 30 days;
- Default of 4 MB/s or 4.000 messages per second;
- The cost is defined per stream per hour and per data in/out per GB.
- Recommended when the throughput is unknown.

The initial number of shards can be defined by:

{: note }
> $$
> NumShards = max(\frac{NumRecordsPerSecond * ItemSizeKb}{1024}, \frac{NumRecordsPerSecond * ItemSizeKb * NumConsumers}{2048})
> $$
>
> \* Item size in KB must be rounded up.

The data is automatically and synchronously replicated across 3 AZs and the data can be replayed. Once in the Stream, the **data cannot be deleted (*immutability*)** and it only leaves the Stream after the ***Retention Period*** which can be 365 days maximum but, by default, it is set to 24 hours. More retention is safer, however, it is also more expensive.


### Records

Records are produced by the *Producers*, which can be:

- Amazon SDK;
- Kinesis Producer Library (KPL);
- Kinesis Agent.
- 3rd Party Libraries, such as Spark, Log4j, NiFi, Kafka Connect, etc.

Each record is formed by:

- ***Partition Key***: a unicode string used for directing the record to a particular *Shard*. If the same key is used multiple times it can cause the *hot partition* issue, when the *shard* is unable to handle all the requests graciously.
- ***Data Blob***: anything up to 1 MB.

After the record enters the *Stream*, the ***Sequence Number*** is added to the record and it works as a unique identifier of that record in that *shard*.

Finally, the following consumers can read from the *shard*:

- Kinesis Client Library (KCL);
- Kinesis Connector Library;
- AWS SDK;
- Managed services like AWS Lambda and Kinesis Data Firehose;
- 3rd Party Libraries, such as Spark, Log4j, NiFi, Kafka Connect, etc.

### Producers

#### SDK

Uses ***PutRecord*** or ***PutRecords*** (batch, faster) APIs. Using PutRecords with different partition keys is the fastest way of using SDK - less HTTP Requests and parallel shards.

Use cases include low throughput or higher latency applications, mobile apps, simple APIs or integrating with managed services such as [AWS Lambda](docs/compute/lambda.html).

Some services integrates with *Kinesis Data Streams* using SDK. CloudWatch Logs is an example.

Retry has to be implemented by the developer.

#### Kinesis Producer Library - KPL

A highly configurable C++/Java Library, recommended for high performance and long-running applications. It can run *synchronously* or *asynchronously* and has an automated retry mechanism.

Has integration with *CloudWatch Metrics*.

Two *batching* features, which are enabled by default, helps reducing the data transfer cost, but increases throughput:

- *Aggregate*: Capacity of storing multiple records into a single record (as long as it stays under 1MB of data blob).
- *Collect*: Ability of writing multiple records into multiple shards with a single *PutRecords* call.

If the application is sensitive to delays, it may be not recommended to use KPL, as the batching efficiency is impacted by the parameter ***RecordMaxBufferedTime*** - 100ms by default - that indicates the amount of time the process will wait to batch the records. More time equals more efficiency but more latency. Less time means less efficiency but also less latency.

KPL records must be de-coded with KCL or helper libraries.

#### Kinesis Agent

A stand-alone Java application for **monitoring files/logs on servers**.

Some features include:

- Checkpointing;
- Retry mechanism;
- CloudWatch monitoring;
- Data pre-processing, like CSV to JSON;
- Routing based on directory/file name;
- It can write from multiple directories to multiple streams.

Ideal for server's logs monitoring.

### Consumers

#### SDK

Records are polled from a shard via a ***GetRecords*** API Call. To send this call, one must provide a *Shard Iterator*, which can be retrieved via a ***getShardIterator*** call.

Given each shard has 2MB total read throughput and each *GetRecords* can return up to 10MB, each call has to wait 5 seconds. In addition, each shard can receive up to 5 GetRecords requests, meaning each consumer will receive less than 400KB/s with 200ms of latency.

#### Kinesis Client Library (KCL)

Library mainly used for reading records produced by the Kinesis Producer Library, as it can de-aggregate that records.

Has ***Shard Discovery***: shares multiple shards with multiple consumers in a single *group*.

Has *checkpointing* feature by using a DynamoDB table. If WCU and RCU are not provisioned properly, KCL may be slowed down by the DynamoDB Table.

#### Kinesis Connector Library

It is a Java application that must be used in an EC2 instance for sending data to S3, DynamoDB, Redshift or ElasticSearch. It was largely replaced by **Kinesis Data Firehose**.

#### AWS Lambda

Lambda Functions can read from the *Kinesis Data Streams* and de-aggregate records produced by the *Kinesis Producer Library*.

It is recommended for light ETLs/reacting from events (sending notifications/e-mails, for example) and can send the data to pretty much anywhere.

The function will receive the records in configurable batches of up to 10.000 records or 6MB of data.


### Troubleshooting

#### **ProvisionedThroughputException**

***ProvisionedThroughputException*** is raised in case a Shard receives more than:

- Writing requests of more than 1MB/s or more than 1.000 messages/s PER SHARD;
- Reading requests of more than 2MB/s or more than 5 transactions/s PER REGULAR SHARD.
- Reading requests of more than 2 MB/s PER SHARD AND PER  CONSUMER if ***Enhanced Fan-Out*** is enabled

Possible solutions are:

- Ensure the chosen *Partition Key* is not causing *Hot Partition*;
- Increase number of *shards*;
- Implement *exponential backoff* and retry the request later;
- Aggregate the messages;
- Compress the messages.

#### ExpiredIteratorException

If while using KCL this exception is raised, increase the WCU in the checkpointing DynamoDB table.

#### KCL is ignoring records

The main cause is some exception occurring in your code that is absorbed by KCL's *processRecords* method, so that the consumer does not stop.

#### KCL is slow

This may occur for some reasons:

- Low number of shards;
- GetRecords API Limit may be too low;
- *processRecords* method too complex and time consuming.
