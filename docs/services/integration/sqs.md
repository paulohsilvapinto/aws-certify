---
layout: default
title: SQS
parent: Integration
grand_parent: AWS Services
last_modified_date: 2024-11-18
---

# Amazon Simple Queue Service - SQS
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

Amazon Simple Queue Service (Amazon SQS) is a fully managed service that  lets you send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be available.

![Amazon SQS](https://d1.awsstatic.com/product-page-diagram_Amazon-SQS%402x.8639596f10bfa6d7cdb2e83df728e789963dcc39.png)

Use cases:

- Decoupling Applications
- Buffering Data

SQS is charged per API request and per network usage.

## Standard Queues

Standard Queues offers low latency (less than 10ms) and no limit on number of messages, producers or consumers.

The message delivery follows the *at least once* principle, so there may be duplicate messages. Also, message order is not guaranteed.

## FIFO Queues

First-In, First-Out queues ensures messages are processed in order and exactly once. They are identified by a ***.fifo*** suffix in their name.

They offer a lower transfer rate with 30.000 messages per second with batching or 3.000 messages per second without batching.

Messages can be deduplicated in a 5 minutes interval if a message **Deduplication ID** is provided.

## Dead Letter Queues - DLQ

When a consumer fails to process a message, the message does not get deleted, which makes it "go back to the queue". To avoid the same messages going back to the queue infinitely (can even get to a point where the queue is stuck in those messages), maybe due to a broken message or a bug in the consumer, we can send these messages to an alternate queue after a defined number of attempts of processing that message. This alternate queue is a regular queue of the **same type of the original queue**, but is known as a **Dead Letter Queue**.

When the issue is identified, there is a feature to **redrive the messages to the source** queue so that they can process successfully.

## Messages

Messages are composed by a Body and the message metadata:

- **Body**: up to 256kb with data type String.
- **Metadata**: key-value pairs, user defined, that are attached to the message.

Whenever a message is sent into the Queue, the producer receives a *message identifier* and a *MD5 hash of the body*. Message delivery can be *delayed* if set to.

Messages can be retained in the queue from seconds to up to 14 days. By default the retention period is 4 days.

Whenever a consumer polls a message (single or batch of up to 10 messages) the message(s) enter a **visibility timeout** period, where the message becomes invisible to other consumers. Ideally, the message should be processed before the time ends and then **deleted** from the queue. Otherwise, other consumer will start processing the same message after the visibility timeout has expired.

{: note }
For sending large messages, it is possible to use a Java Library called **SQS Extended Client**. It uses S3 for storing the real message, while SQS receives only a pointer to the S3 file. However, it is not really recommended.

## Security

Encryption in-flight with HTTPS endpoint and at rest with Server Side Encryption using KMS (which can handle *Customer Master Keys - CMKs*).
