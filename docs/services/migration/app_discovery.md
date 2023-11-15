---
layout: default
title: Application Discovery
parent: Migration
grand_parent: AWS Services
---

# AWS Application Discovery Service
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

The AWS Application Discovery Service is used for mapping resource information of on-premises servers and its report is extremely handy for planning on-premises to cloud migrations. The reports can be visualized on *AWS Migration Hub*.

There are two ways to run the discovery service:

- Agent-based: the best option when it comes to gathering information, but requires an agent.
- Agentless, using a connector: gathers less information when compared to the other method, but does not require an agent running on your servers. 

![AWS App Discovery](https://d1.awsstatic.com/products/application-discovery-service/Application_Discovery_service-HIW.55d6ec4b513f76249d277687ce67c6a6f6841287.png)
 