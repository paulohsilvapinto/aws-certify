---
layout: default
title: Elastic Kubernetes Service
parent: Containers
grand_parent: AWS Services
last_modified_date: 2023-11-29
---

# Elastic Kubernetes Service
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

Amazon Elastic Kubernetes Service (Amazon EKS) is a managed Kubernetes service to run Kubernetes in the AWS cloud.

Amazon EKS automatically manages the availability and scalability of the Kubernetes control plane nodes responsible for scheduling containers, managing application availability, storing cluster data, etc. For a multi-region application, deploy one EKS cluster per region.

*Cloudwatch Container Insights* can be used for log and metrics monitoring.

## EKS vs ECS

EKS is an alternative to ECS with different APIs and flexibility. Some key differences are:

- ECS is easier to learn (and therefore more straightforward to deploy applications) but gives less freedom for managing the underlying resources;
- EKS offers the possibility of indicating where to place the containers (pods) thanks to the nodes' affinity and taints mechanisms. 
- EKS offers more options for monitoring.
- ECS is AWS proprietary. Kubernetes is open-source (and therefore cloud-agnostic)

## Launch types

EKS, just like [ECS](./ecs.html), can be deployed with [EC2 instances](/docs/services/compute/ec2.html) or Fargate serverless mode. However, there are two options for EC2 instances:

### Managed Node Groups

EKS creates and manages the EC2 instances (Nodes) on our behalf, and the nodes are part of an *Auto Scaling Group* managed by EKS. The advantage of this mode is that it supports on-demand and spot instances.

### Self-managed Node Groups

Similar to the *Managed Node Groups*, but now the EC2 instances must be provisioned and managed by us. The advantage of this mode is that it allows you to hand-pick the instance type most appropriate for your applications and allows the usage of custom AMIs.

## Load balancing

The same load balancers for ECS are available for EKS, and the use cases are the same.

## Storage

EKS supports [EBS](/docs/services/storage/ebs.html), [EFS](/docs/services/storage/ebs.html), and FSx.

Fargate mode only supports EFS.

For using a storage device with EKS, a *StorageClass* manifest (a YAML file) is required, and then EKS will leverage a **Container Storage Information (CSI)** compliant driver, which will allow the container to interact with the storage devices.