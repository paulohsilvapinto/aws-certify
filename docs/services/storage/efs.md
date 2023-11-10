---
layout: default
title: EFS - Elastic File System
parent: Storage
grand_parent: AWS Services
---

# EFS - Elastic File System
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

- It is a managed Network file system (NFS), but differently from the EBSs, they work on **multiple AZs** and must be set inside of a VPC Security Group for access control.
- **More expensive** than EBS, but it is **paid per use**. No capacity provisioning here - file system will scale automatically.
- Highly available (multi-AZ) and highly scalable.
- Files are **encrypted at rest with KMS**.
- An EFS may be attached to multiple EC2 instances and an instance may have multiple EFSs attached to it.
- Uses NFSv4.1 protocol.
- Recommended for web serving, data management/sharing, etc.
- They are only compatible with Linux-based images. **No compatibility with Windows images**.
- AWS Lambda can integrate with EFS.

## Performance

It is possible to set the Performance Mode and the Throughput Mode.

### Performance Mode

Performance Mode is set the EFS creation time and can be:

#### General Purpose

- **Default mode for EFS**. This is the recommended mode by AWS for all file systems.
- Recommended for **latency-sensitive** scenarios such as web serving.

#### Max I/O

- **Higher throughput** and parallelism capabilities, but also **higher latency**.
- Recommended for Big data processing, media processing etc.
- **Not compatible with Elastic throughput mode** as the throughput scales automatically in that mode.

### Throughput Mode

There are three types of Throughput mode:

#### Bursting

- Throughput that **scales with the amount of storage** in your file system.

#### Provisioned

- Throughput is set by you.
- Recommended when you know how will be your wokload requirements.

#### Elastic

- Recommended mode by AWS.
- **Scales with the workload** in your file system. Recommended when there are spiky or unpredictable workloads.

## Storage Classes

There are four different storage class and **files on a single EFS can be on diffent tiers (IA or non-IA)**. It is also possible to set **Lifecycle management rules**. The classes are:

 - **EFS Standard**: Data is replicated across multiple AZs. Ideal for frequently accessed files.
 - **Standard IA**: Infrequent access. There are lower storage cost, but file retrieval is paid.
 - **EFS One Zone** : Data is replicated within a single AZ. Suitable for development environment. *Not suitable for production environment*.
 - **EFS One Zone IA**: over 90% cheaper than the rest.