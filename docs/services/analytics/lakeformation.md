---
layout: default
title: AWS Lake Formation
parent: Analytics
grand_parent: AWS Services
last_modified_date: 2024-11-07
---

# AWS Lake Formation
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

AWS Lake Formation centralizes permissions management of your data and makes it easier to share across your organization and externally.

![AWS Lake Formation](https://d1.awsstatic.com/diagrams/Lake-formation-HIW.9ea3fab3b2ac697a42ae7a805b986278ffd4f41e.png)

It is built on top of Glue and can:

- Start and monitor data flows;
- Define transformation jobs;
- Encrypt and manage encryption keys;
- Manage Access Control - from top Level to Granular Level: Rows, Columns, Cell-Level (with row and column filter applied simultaneously)  and SQL Operations;
- Data Auditing.

## ACID Compliance

You can create and manage transactional tables using open source formats such as [Apache Iceberg](https://iceberg.apache.org/), [Apache Hudi](https://hudi.apache.org/) or [Linux Foundation Delta Lake](https://delta.io/).

{: note }
**Governed tables** are deprecated

## Cross-account permissions

If the grantee account is part of the same organization, the access is made available immediately.

If the grantee account is not part of the same organization, an invite is sent via **AWS RAM - Resource Access Manager**, which can be accepted or declined by the *data lake administrator* in the grantee account.

## Cost

Lake Formation is free. However, any service used under the hood, such as [S3](docs/storage/s3.html), [EMR](docs/analytics/emr.html), [Athena](docs/analytics/athena.html), etc are not.
