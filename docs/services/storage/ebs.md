---
layout: default
title: EBS - Elastic Block Store
parent: Storage
grand_parent: AWS Services
---

# EBS - Elastic Block Store
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

EBS is a network drive (some latency should be expected) that can be attached/dettached anytime to an EC2 instance quickly - A *"Virtual Pen Drive"*. It has to be provisioned (size in GBs and IOPS), but can be increased later, and allows data to be persisted even after the instance is terminated.

**They are bound to an specific availability zone. They can only be attached to EC2 instances in the same AZ as they are. Data can be moved to other AZs only via snapshots.**

{: .important }
**DELETE ON TERMINATION: By default the ROOT volume is set to delete on termination, but any additional volume by default is not set to delete.** This behaviour can be changed using the Console when creating the EC2 instance. **So, to preserve the root storage, you need to disable this option!**

An EBS may be attached to a single EC2 instance (except multi-attach IO1/IO2, if EC2 instance is using the *Nitro system*), and an instance may have multiple EBSs attached to it.

## Volume Types

There are four different type of EBS Volumes:

- **GP2**: *General Purpose SSD*. Provides a balanced price and performance capability. It can range from 1GB up to 16GB.
- **IO1**: *Provisioned IOPS SSD*. High-performance and low-latency SSD ideal for storing critical data (ex. RDBMS).
- **ST1**: *Throughput Optimized HDD*. Low-cost HDD that provides a consistent throughput ideal for frequently accessed data (ex. Big Data / Data Warehouse).
- **SC1**: *Cold HDD*. Extremely low-cost HDD, ideal for non-frequently accessed data and for minimal cost.

{: .note }
> In general:
> - **SSD Volumes**: ideal for randomic and small access. Only SSD disks can be set as a *BOOT disk*.
> - **HDD Volumes**: Sequential and big access. Cannot be used as a *BOOT disk*.

## Snapshots

The snapshots are *incremental-only* - the last version always contain all the data - and are stored on S3 (not visible). If the Volume is encrypted, the snapshots are also encrypted.

It is recommended to execute a snapshot when there is low traffic and/or to detach the instance first, as snapshots consume IO.

Snapshots are useful for backup, for moving data to another AZ, and for creating AMI (Amazon Managed Image) from them.

*Amazon Lifecycle Manager* allows the snapshot automation.

## RAID Configurations

RAID are ways of combining multiple Physical Disks for better storage, security and/or performance. 

Even though for EBS Volumes only RAID0 and RAID1 are recommended, the RAID options available are dependent exclusively on the Operating System.

### RAID0

Combines two physical disks into one final logical disk for combined storage and better IOPS as the traffic is split between the disks.

### RAID1

Used for increased fault-tolerancy by mirroring one physical disk into another. Increases the network usage, potentially increasing latency.

