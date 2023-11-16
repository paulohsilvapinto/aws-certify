---
layout: default
title: AWS Transfer Family
parent: Migration
grand_parent: AWS Services
---

# AWS Transfer Family
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

The Transfer Family is a fully-managed service that allows [S3](docs/services/storage/s3.html) and/or [EFS](docs/services/storage/s3.html) data to be accessed via *FTP*, *FTPS*, *SFTP*, or *AS2* protocols.

![Transfer Family](https://d1.awsstatic.com/cloud-storage/product-page-diagram_AWS-Transfer-Family_HIW-Diagram.4af0b3b19477f22bc7e37995c43cf833b6db0ce9.png)

It is scalable, reliable, and highly available (multi-AZ). Allows integration with Authentication systems such as *Microsoft Active Directory*, *Amazon Cognito*, *LDAP*, etc.

It is possible to use _AWS Route 53_ to attach a custom DNS to the transfer endpoint.