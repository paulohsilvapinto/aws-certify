---
layout: default
title: EC2
parent: Compute
grand_parent: AWS Services
---

# Elastic Compute Cloud - EC2
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

Amazon Elastic Compute Cloud (Amazon EC2) is a single-region web service that provides secure, resizable compute capacity in the cloud. It works as a *Virtual Machine*, with each instance being based on an *Amazon Machine Image - AMI*.

Each instance can be attached to one or more Security Groups, which works similarly to a *Firewall*. **By default, all inbound traffic is blocked, and all outbound traffic is allowed.** To access the instance via SSH, you must enable access to port 22.

EC2 instances are billed by the *Pay as you go* model, charged every second.

{: .note }
By default, the root disk of the EC2 instance is deleted when the instance is terminated.

## Purchasing Options

The following purchasing options are available:

- **On-Demand**: Pay by the second with no upfront cost. Recommended for quick or unpredictable workloads that cannot be interrupted.
- **Reserved**: Commit to a one or 3-years term for a specific instance configuration, including instance type and Region. In the long term, they are up to 75% cheaper than on-demand instances. Recommended for stable applications such as a Database. There are three options available:
  - **Reserved Instance**: a specific instance is reserved. It is not possible to change it.
  - **Convertible Reserved Instance**: instances can be modified, but is more expensive than the *Reserved Instance* type.
  - **Scheduled Reserved Instance**: the instance is reserved only during a time period, like from 08:00 to 12:00. It is the cheapest option between the Reserved options.
- **Savings Plans**: Commit to a one or 3-years term for a particular amount of usage, in USD per hour.
- **Dedicated Hosts**: Pay for a physical host that is fully dedicated to running your instances, and bring your existing per-socket, per-core, or per-VM software licenses to reduce costs.
- **Dedicated Instances**: Pay by the hour per instance (not per host), for instances that run on single-tenant hardware.
- **Spot**: Use an unused EC2 instance for a low-cost option (the cheapest option). However, the instance can be lost anytime if the instance price goes beyond the max value set by you, potentially causing a loss of data/processing. AWS gives extra minutes after the price threshold is passed. Ideal for fault-tolerant processes like Machine Learning, Big Data processing, etc. It is recommended to have a checkpoint mechanism for restarting the process when needed.
- **Spot Fleet**: Use a set of Spot Instances plus, optionally, on-demand instances. To use this option, you must set some launch pools containing Instance Type, OS, AZ, etc. Depending on the number of machines and capacity required, AWS will choose which combination of launch pools to use. There are three strategies for choosing the pools: lowest price, capacity-optimized, or diversified.

## Instance Types

These are the main instance family types available:

- **R**: For RAM-intense usage. In-memory cache for example.
- **C**: For CPU-intense usage. Examples: Batch Processing, HPC, etc.
- **M**: For balanced applications (M = Medium). Example: Web Apps and code repositories.
- **IO**: For IO-intense (read and write of large data) applications. Example: Database
- **P/G**: For GPU-intense applications. Examples: Video processing, Deep Learning, etc.
- **H**: High-Disk Throughput (HDD)
- **T2/T3**: *"Burstable instances*: while there are *burst credits* available, the performance is good. After the credits are consumed, a downgrade in performance can be observed. When the instance is not in *burst mode*, the credits are accumulated. Recommended for spiky usage.
- **T2/T3 Unlimited**: Similar to *T2/T3*, but when the credits are gone, there is no loss of performance. However, each additional credit will be charged.

## AMI - Amazon Machine Image

AMI is a snapshot of one or more [EBS](https://paulohsilvapinto.github.io/aws-certify/docs/services/storage/ebs.html) Disk, and can be used as a *boot* image for an EC2 Instance. AWS already has some managed AMI's with Linux, Windows, MacOS, etc.

They are region-bound, not global, but copying them to another region is possible.

It is not possible to install an encrypted AMI unless you have the encryption key.

## Placement Groups

There are three placement strategies available:

- **Cluster**: instances are grouped in a low-latency, single-AZ configuration. It offers high performance but no fault tolerance as everything is in the same AZ.
- **Spread**: High-resilience by placing the instances in different computer racks and AZs. Recommended for highly-available applications or when faults must be isolated from other instances.
- **Partition**: Each partition is a server rack inside of an AZ. This model allows up to 7 partitions per AZ, and up to 100 instances per group. A failure in a partition may affect multiple instances inside the partition but will not affect other partition. Recommended for distributed workloads such as HDFS, Kafka, HBase, etc.

## Elastic IP

Every time an instance is restarted, its public IP address changes.

To keep the same public IP address every time the instance starts, use *Elastic IP*.

Elastic IP is also useful for failover. If the instance fails, simply re-attach the same IP to another instance. However, the best practice is to use Route 53 for a custom DNS and/or to use a Load Balancer.

## Instance Metadata

Every instance has access to the address **http://169.254.169.254/latest/meta-data**, which can provide metadata about the current instance.

## Bootstrapping

Bootstrapping means running a *bash* script before the instance is available to be used. Some use cases include installing updates, starting services, downloading files, etc. 

This script is executed as *root*, only once after the instance is provisioned but before being available to be used.

## EC2 Hibernate

There are three ways to stop an EC2 instance:

- **STOP**: data is kept on the EBS volumes, including the primary one, for the next start.
- **TERMINATE**: the instance is deleted, and, by default, the root disk is also deleted.
- **HIBERNATE**: the RAM data is saved on the disk for a faster boot.

Hibernate requires an encrypted EBS volume and is ideal for long processes or for services that take too long to start.

Some limitations are:

- Only compatible with instance types C, M, and R;
- The instance may have up to 150GB;
- The root volume must be encrypted.
- Not available for Spot instances.
- An instance can hibernate for a maximum of 60 days.

## Elastic Network Interface - ENI

ENI is a logical *VPC* component representing a virtual *Network Interface Card*. They allow EC2 instances to connect to the internet (but can be attached to other services as well) and they are *AZ+Subnet-bound*

An EC2 instance can have more than one ENI and, therefore, be part of more than one subnet. Except for the primary ENI, called *eth0*, which cannot be detached from their instance, the secondary ENIs can move to other instances, and it is ideal for masking failures.

The ENIs can have:

- A primary private IPv4 and one or more secondary private IPv4;
- One Elastic IP (IPv4) per private IPv4;
- One or more Security Groups
- One MAC address.

The interface attachment can be:

- **Cold**: when the interface is attached when the EC2 instance is launching;
- **Warm**: when the interface is attached when the EC2 instance is stopped;
- **Hot**: when the interface is attached when the EC2 instance is running.

## Elastic Fabric Adapter - EFA

They are a better Network Interface when compared to ENIs, but are more expensive and only compatible with Linux OS. They are recommended for [HPC](https://paulohsilvapinto.github.io/aws-certify/docs/services/glossary.html) and distributed computation.

They use the Message Passing Interface - MPI standard.

They work by bypassing the Linux OS for lower latency and more reliable transport.