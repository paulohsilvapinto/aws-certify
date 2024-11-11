---
layout: default
title: Amazon EMR
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-11
---

# Amazon EMR
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

EMR stands for *Elastic Map Reduce* and is a managed Hadoop framework running on [EC2](docs/compute/ec2.html) instances. Charged by hour of EMR plus [EC2](docs/compute/ec2.html) instance costs.

Compatible with EMR notebooks for interactive python queries.

EMR Clusters can be serverless - altough you still need to define some properties of the nodes such as RAM size.

EMR can also run on Kubernetes via [EKS](docs/containers/eks.html). A big advantage is that you can share the EMR data with different Kubernetes pods with different applications.

## EMR Cluster

A *cluster* is a collection of [EC2](docs/compute/ec2.html) instances that communicates with each other, and each instance is called *Node*.

There are three types of nodes:

- ***Master Node (or Leader Node):*** It is a mandatory single [EC2](docs/compute/ec2.html) instance, responsible for managing the cluster and distributing and monitoring tasks. However, if it is the only node, it will also be responsible for performing the tasks itself and storing data.
- ***Core Node:*** Executes tasks and stores data in the *Hadoop Distributed File System (HDFS)*. A multi-node cluster has at least one Core Node for storing the data. The cluster can be up-scaled or down-scaled. However, as it stores data in the HDFS, it may cause data loss. If a Core Node fails, another one is added automatically.
- ***Task Node:*** Similar to a Core Node, but without storing data in the HDFS, making it the ideal choice for temporarily scaling the cluster. Spot instances tend to be a great choice for Task Nodes. Task nodes are not mandatory in a cluster and can be added/removed *on the fly* for increasing/reducing processing capacity.

## Usage

Frameworks and applications are chosen at cluster launch, so that EMR can install all required dependencies automatically.

Jobs can be defined and initiated via *CLI*, by connecting directly to the *Master Node*, or via *AWS Console*.

There are two ways of running a Cluster:

- ***Transient Cluster:*** Cluster is automatically terminated as soon as the job is completed.
- ***Long-Running Cluster:*** The cluster is always up and running, always ready for a new job or for serving data from the HDFS (serving as a Data Warehouse). Using reserved [EC2](docs/compute/ec2.html) instances is recommended. Auto-scaling and Instance Fleet is also a nice choice. Has termination protection on by default and auto-termination off, which is useful for backing up the data before the cluster is terminated..

{: note }
Termination Protection does not block instances from being terminated if it is required for auto-scaling or if the [EC2](docs/compute/ec2.html) instance is a spot instance.

## Storage

There are several options for storage in EMR:

### HDFS (Hadoop Distributed File System)

Files are broken in 128 megabytes parts and distributed accross the Core Nodes.

Performance is its greatest advantage, as tasks can be distributed in the same instances as the required data is already located.

On the other hand, availability is its downside, as it is pretty much an *ephemeral storage*: if the instance is lost or terminated, the data will no longer be accessible and will be lost.

### EMRFS

Similar to HDFS, but instead of storing in the instance itself, the data is stored in [S3](docs/storage/s3.html). In this case, the cluster may still benefit from the internal HDFS for intermediate processing.

In the past it was required to enable the option ***EMR Consistent View*** for ensuring the data would be consistent for every Node. This was guaranteed by a DynamoDB table storing metadata of the [S3](docs/storage/s3.html) objects. However, it is no longer needed as [S3](docs/storage/s3.html) is now Strongly Consistent.

### Local File System

It is not the best option for long-term storage, but may be useful for storing transient/temporary data, such as buffer, cache or raw data.

### EBS for HDFS

EMR will automatically allocate a 10GB SSD EBS volume attached as *Root Device*, but it is possible to add more volumes **before the cluster has started**.

It is a good option for reducing costs, but contrary to pure [EC2](docs/compute/ec2.html) instances, **EBS volumes attached to EMR Clusters are terminated when the cluster is terminated** - making it an *ephemeral storage*.

If a EBS volume is detached, EMR will assume the disk has failed and will replace with another one.

It is not possible to do Snapshots of disks on EMR and it is only possible to use an encrypted EBS volume with a custom AMI.

## Integration

EMR integrates with several AWS Services:

- [EC2](docs/compute/ec2.html): EMR is built on top of [EC2](docs/compute/ec2.html);
- VPC: Private Network where the cluster is located;
- [S3](docs/storage/s3.html): to store input/output data;
- CloudWatch: for monitoring cluster's health and performance metrics, and for configuring alarms;
- IAM: for handling permissions;
- CloudTrail: for auditing cluster's usage;
- AWS Data Pipeline: for scheduling and starting the cluster.
