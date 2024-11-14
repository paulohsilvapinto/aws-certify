---
layout: default
title: Amazon Managed Service for Apache Flink
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-14
---

# Amazon Managed Service for Apache Flink
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

A managed service for deploying *Flink* Applications that run over a data streaming. The *Flink* Application can be written with Java, Python, Scala and SQL (by using the Table API) and they are stored in [Amazon S3](docs/storage/s3.html) for later *Flink* usage.

It can receive data from [Kinesis Data Streams](docs/analytics/kinesis.html) or from [Amazon MSK](docs/analytics/msk.html). As output, it can deliver data to [Amazon S3](docs/storage/s3.html), [Kinesis Data Streams](docs/analytics/kinesis.html) and [Kinesis Data Firehose](docs/analytics/kinesis.html#kinesis-data-firehose).

It is basically a replacement for what was the Kinesis Data Analytics, which already used Apache Flink under the hood.

Use cases include:

- Streaming ETL
- Continuous Metric Generation
- Responsive Analytics
