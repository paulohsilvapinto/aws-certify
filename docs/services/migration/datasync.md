---
layout: default
title: AWS DataSync
parent: Migration
grand_parent: AWS Services
last_modified_date: 2023-11-28
---

# AWS DataSync
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

AWS DataSync is a secure, online service that automates and accelerates moving data between on-premises and AWS Storage services. 

![AWS DataSync](https://d1.awsstatic.com/Digital%20Marketing/House/Editorial/products/DataSync/Product-Page-Diagram_AWS-DataSync_On-Premises-to-AWS%402x.8769b9dea1615c18ee0597b236946cbe0103b2da.png)

DataSync can copy files and objects between Network File System (NFS), Server Message Block (SMB), Hadoop Distributed File Systems (HDFS), self-managed object storage, AWS Snowcone, S3 (**any storage tier**), EFS, and FSx. 

DataSync **cannot** migrate data from databases. For this, use [AWS DMS](./dms.html) instead.

It does not provide continuous replication, providing scheduled replication by hour, day, or week instead.
For *AWS* to *AWS* transfers, no agent is required. Otherwise, a *DataSync Agent* is required.

{: .important }
DataSync can copy the data, the **metadata**, and the **file permissions** of the copied files.

## AWS Snowcone

AWS Snowcone is a device to store the data to be transferred that already comes with the *DataSync Agent* pre-installed. Once the data is loaded into the device, it is shipped back to the Amazon Data Center. It is useful when there is limited or no access to the internet.