---
layout: default
title: AWS Batch
parent: Compute
grand_parent: AWS Services
---

# AWS Batch
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

AWS Batch is a fully managed batch computing service that plans, schedules, and runs your containerized batch or ML workloads across Amazon ECS, Amazon EKS, AWS Fargate (Serverless and recommended for most tasks), and Spot or On-Demand EC2 Instances.

Jobs can be scheduled by CloudWatch Events or triggered by Amazon EventBridge, Lambda Function, etc.

Use cases include:

- ML model training;
- Genome Sequencing;
- Any non-ETL Batch processing;

## Components

AWS batch is formed by four components:

- **Compute Environments**: Defines the computing resources that will be available for a specific job queue. You can select the orchestration type (Fargate, [EC2](ec2.html), or EKS), min and max vCPU, managed or unmanaged, instance type, and so on.
- **Job Queues**: Queues for receiving the jobs. Each queue can be attached to up to three computing environments, and it is possible to set the queue priority as well, where queues with higher priority will be processed first. By default, the queues are processed in a FIFO manner, but it can be changed with a scheduling policy.
- **Job Definitions**: Defines what is needed for a job to be executed: docker image location, commands to be executed, what to do in case of failure, how much vCPU is needed, environment variables, etc.
- **Job**: Creates a job using the job definition and sends it to the job queue specified by you. Some parameters from the job definition can be overridden.

## Orchestration Types

### Fargate

Fargate launches and scales the compute to closely match the resource requirements that you specify for the container. It is quicker to spin up than an EC2 instance, and you do not need to worry about provisioning resources. It is the recommended orchestration type for most scenarios.

### EC2 Instances

It is recommended for when there is a requirement for certain instance types (particular processors, GPU, architecture, AMI, etc.) or for huge workloads (more than 16 vCPUs and/or 120GB).

EC2 instances can be on-demand or spot instances. In case of spot instances, it is possible to set the maximum percentage of the on-demand price that you consider worth using spot instances over on-demand instances.

### EKS

EKS is recommended when you want to leverage Kubernetes while still having the fast, scalable, and secure AWS Infrastructure behind it.
