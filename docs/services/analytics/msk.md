---
layout: default
title: Amazon MSK
parent: Analytics
grand_parent: AWS Services
math: katex
last_modified_date: 2024-11-11
---

# Amazon Managed Streaming for Apache Kafka (Amazon MSK)
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

A fully managed service for Streaming Data using Kafka cluster. The cluster will contain many broker nodes replicating data with it other and a single Zookeeper node per Availability Zone. The cluster can be deployed in up to three Availability Zones and inside a VPC.

Data is stored on EBS volumes and data is encrypted by default, be it at rest on in-transit.

***MSK* is a great alternative to Kinesis, as it is much more configurable.** For example, just like Kinesis, the default message size is 1MB. However, different from Kinesis, it can be changed to up to 10MB. It is also possible to choose the number of AZs, VPC, broker instance type, number of brokers per AZ, size of the EBS volumes (from 1 to 16GB), etc.

*Producers* must be developed and *Consumers* can be developed or benefit from ***MSK Connect***. *Producers* will write data into a topic and the consumers must poll data from a topic.

It is also possible to setup **MSK Serverless**, where you don't need to provision or manage capacity, as it will be done automatically, scaling compute and storage. It is only required do define the topics and the partitions. Pricing on *MSK Serverless* is per cluster, per partition and per data in, out and stored.

## MSK Connect

Kafka Connect is a framework for polling data from the Kafka cluster and storing somewhere else.

MSK Connect is a Managed, Auto Scalable, Kafka Connect Workers. By deploying Kafka Connect connectors as a plugin to MSK Cluster, you can create MSK Connect Workers to pull topic data and write to, for example, [Amazon S3](docs/storage/s3.html), [Redshift](docs/database/redshift.html), [OpenSearch](docs/analytics/opensearch.html), etc.

## Authentication and Authorization

Defines who can write/read data to/from which topics. It can be:

- Authentication: Mutual TLS + Authorization: Kafka ACLs;
- Authentication: SASL/SCRAM + Authorization: Kafka ACLs;
- Authentication and Authorization: IAM Access Control.

## Monitoring

Monitoring can be done via:

- CloudWatch metrics: basic monitoring, enhanced monitoring or topic-level monitoring;
- Prometheus: An Open source software for monitoring and alerting;
- Broker Log Delivery: log can be delivered to [S3](docs/storage/s3.html), CloudWatch Logs and [Kinesis Data Streams](docs/analytics/kinesis.html).

## Kinesis Data Streams x Amazon MSK

|                  | Kinesis Data Streams          | Amazon MSK                                     |
| ---------------- | ----------------------------- | ---------------------------------------------- |
| **Configurable** | Slightly Configurable         | Highly Configurable                            |
| **Concepts**     | Data Streams with Shards      | Kafka Topics with Partitions                   |
| **Scaling**      | Shard Splitting and Merging   | Can only add partitions to a topic             |
| **Encryption**   | TLS in-flight and KMS at-rest | Can be Plain Text in-flight or same as Kinesis |
| **Security**     | IAM Policies | MutualTLS or SASL/SCRAM with Kafka ACLs or IAM Access Control   |
