---
layout: default
title: Elastic Container Service
parent: Containers
grand_parent: AWS Services
last_modified_date: 2024-11-05
---

# Elastic Container Service
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

Amazon Elastic Container Service is a fully managed container orchestration service for efficiently deploying, managing, and scaling containerized applications in the cloud or on-premises.

Containers can be based on public or private docker images from any registry, including [Amazon ECR](./ecr.html)

## Terminology

ECS runs containerized applications.

A *Container* is a piece of software that contains the application and all of its dependencies. They are created from a read-only template called *image*, which in turn is created from a text file called *Dockerfile*.

*ECS* also needs a *task definition*, which is a JSON file containing the parameters (for example, which ports to use) and the containers required for a specific application.

A *task* is the instantiation of a *task definition*, and it can be deployed as a *service* (long-running processes or processes that must be kept alive) or a simple *task* (quick/batch processes).

The *ECS scheduler* is responsible for monitoring and launching new containers whenever needed based on your requirements. It does it by communicating with the *ECS Agents* that run on each container instance.

## Launch Types

There are three distinct ways of launching containers with *ECS* - via [EC2 instances](/docs/services/compute/ec2.html), via [Fargate](./fargate.html), or using on-premises VMs/servers.

### EC2 Instance

*EC2 instance* mode allows you to choose the instance type, the number of instances, and many more. However, you are responsible for managing and updating the instances, and an agent has to be installed on each instance so they can be registered in the cluster.

### Fargate

Fargate is a serverless, pay-as-you-go compute engine. Being serverless, you don't need to manage servers, handle capacity planning, or pay when the containers are not running. It's also easier to scale.

### On-premises

*Amazon ECS Anywhere* allows you to register an external instance to the Amazon ECS cluster.

## IAM Roles

### Instance Profile

The Instance profile is a set of permissions used by the ECS Agent and is only available for EC2 Launch mode. The instance profile will allow or deny the ECS agent to communicate with ECS and ECR and publish logs into Cloudwatch.

### ECS Task Role

They can be set via the parameter `taskRoleARN` in the *task definition* (JSON file). They grant or deny access for the specific tasks to other AWS services, such as S3, DynamoDb, Lambda, etc.

## ECS Setup

There are two ways of setting up ECS in an EC2 instance:

- via installing an ECS Agent with a Config File in the EC2 instance;
- via an AMI Linux ECS-ready image.

In both scenarios, a config file will be created under `/etc/ecs/ecs.config`. Some important parameters are:

- **ECS_CLUSTER**: identifies to which ECS Cluster this EC2 instance belongs to;
- **ECS_ENGINE_AUTH_DATA**: used for pulling images from a private Docker Image Registry;
- **ECS_AVAILABLE_LOGGING_DRIVERS**: for logging data into Cloudwatch;
- **ECS_ENABLE_TASK_IAM_ROLE**: to enable IAM roles for ECS tasks.

## Load Balancing

*ECS* supports any type of load balancing:

- **Application Load Balancer**: recommended for pretty much any use case: more resilience (even with a single EC2 thanks to ALB "port mapping"), optimize CPU/memory usage and allows rolling updates with no uptime impact. ALB's feature "port mapping" allows multiple Task instances to run on the same EC2 instance: as the containers are in the same machine, each container will have a unique port number and, in turn they can all be mapped to a single ALB port, making it transparent to the users.
- **Network Load Balancer**: recommended only for high throughput/high-performance scenarios or with AWS Private Link.
- **Classic Load Balancer**: not recommended for any scenario.

## Data Persistence

Containers cannot persist data. However, it is possible to mount an [EFS Disk](/docs/services/storage/efs.html) to multiple *ECS Tasks*, independent from its launch type, which is ideal for multi-AZ data sharing and persistence.

{: .important }
S3 cannot be mounted as a file system.
