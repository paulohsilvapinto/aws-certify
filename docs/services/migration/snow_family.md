---
layout: default
title: AWS Snow Family
parent: Migration
grand_parent: AWS Services
---

# AWS Snow Family
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

The AWS Snow family provides physical devices for offline data migration to *or* from AWS, but from AWS to AWS (for this scenario, there are other options, such as [S3 Replication](/docs/services/storage/s3.html#s3-replication)). They are useful when there is limited / no / shared internet connectivity, and the data would take more than a week to be migrated.

It is reliable, secure, and tamper-free. Data is encrypted with KMS, and it can be mounted as an NFS (*Network File System*) device.

There are three main options in the Snow Family:

- **Snowcone**: more compact and lightweight of the options. Available with SSD or HDD. Does not come with a battery. It is useful when a *Snowball* device is too much or too big. It is also ready to be used with [AWS DataSync](datasync.html).
- **Snowball**: used to migrate TB or PBs of data. Available with SSD or HDD, as Storage Optimized or Compute Optimized, with or without GPU.
- **Snowmobile**: It's a truck that supports up to 100PB of data. If the migration size is more than 10PB, *Snowmobile* is the go-to option.

## Sonwball on Edge

Snowball on Edge can be used when you have a large backlog of data to transfer or if you frequently collect data that needs to be transferred to AWS and your storage is in an area where high-bandwidth internet connections are not available or cost-prohibitive.

You can also use Snowball Edge to run edge computing workloads, such as performing local analysis of data on a Snowball Edge cluster and writing it to the Amazon S3-compatible endpoint. 

It can spin up EC2 Instances and execute Lambda functions (using *AWS Iot Greengrass*) inside of the device.

Snowball Edge is pre-configured and does not have to be connected to the internet, so processing and data collection can take place within isolated operating environments.

## From Snowball to S3 Glacier

Currently, there is no way for unloading Snowball Data directly into S3 Glacier.

The data will automatically be unloaded on S3 Standard Tier. One alternative is to create a Lifecycle rule to move the data to the S3 Glacier tier.

## AWS OpsHub for Snow Family

OpsHub is a user-friendly tool that can be used to manage the Snow devices and local AWS services.

Capabilities:

- Dashboard for device management;
- Unlocking and configuring single or clustered devices;
- Transferring files;
- Launching and managing instances running on Snow Family devices.
