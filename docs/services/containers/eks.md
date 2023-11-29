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

EKS, just like [ECS](./ecs.html), can be deployed with [EC2 instances](/docs/services/compute/ec2.html) or Fargate serverless mode.

*Cloudwatch Container Insights* can be used for log and metrics monitoring.

## EKS vs ECS

EKS is an alternative to ECS with different APIs and flexibility. Some key differences are:

- ECS is easier to learn (and therefore more straightforward to deploy applications) but gives less freedom for managing the underlying resources;
- EKS offers the possibility of indicating where to place the containers (pods) thanks to the nodes' affinity and taints mechanisms. 
- EKS offers more options for monitoring.
- ECS is AWS proprietary. Kubernetes is open-source (and therefore cloud-agnostic)