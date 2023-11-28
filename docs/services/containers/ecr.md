---
layout: default
title: Elastic Container Registry
parent: Containers
grand_parent: AWS Services
last_modified_date: 2023-11-28
---

# Elastic Container Registry
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

Amazon Elastic Container Registry (Amazon ECR) is a fully managed container image registry. It is integrated with [ECS](./ecs.html), [EKS](./eks.html), and the docker CLI.

![ECR How it works](https://d1.awsstatic.com/legal/AmazonElasticContainerRegistry/Product-Page-Diagram_Amazon-ECR.2f9e7f26ef78f4dc6f058f7eeb07cf696f6951c1.png)

Images can be public or private, and they are stored in an S3 bucket, which ensures high availability, durability, and data encryption.

IAM policies control who and what can access each image.

Some features are:

- Lifecycle policies help with managing the images;
- Image scanning helps in identifying vulnerabilities. Each repository can be configured to **scan on push**;
- Cross-Region and cross-account replication;
- Image versioning, image tagging, etc.
