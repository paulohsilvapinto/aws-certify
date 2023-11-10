---
layout: default
title: EBS - Elastic Block Store
parent: Storage
grand_parent: AWS Services
---

# EBS - Elastic Block Store
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## General

- Network drive that can be attached to an EC2 instance - "Virtual Pen Drive". 
- Being a network drive, some latency should be expected.
- Allow data to be persisted even after instance is terminated.
- **They are bound to an specific availability zone. They can only be attached to EC2 instances in the same AZ as they are. Data can be moved to other AZs only via snapshots.**
- Can be detached and attached to other instances quickly.
- Needs a provisioned size (GBs) and IOPS, but can be increased later.
- **DELETE ON TERMINATION: By default the ROOT volume is set to delete on termination, but any additional volume by default is not set to delete.** This behaviour can be changed using the Console when creating the EC2 instance. **So, to preserve the root storage, you need to disable this option!**
- An EBS may be attached to a single EC2 instance (except multi-attach IO1/IO2), and an instance may have multiple EBSs attached to it.
