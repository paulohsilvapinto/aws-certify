---
layout: default
title: Amazon QuickSight
parent: Analytics
grand_parent: AWS Services
math: katex
last_modified_date: 2024-11-18
---

# Amazon QuickSight
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

A user-friendly, serverless, visualization tool that works on browsers or mobiles, with the following capabilities:

- Build and Analyze Visualizations;
- Build Paginated Reports;
- Ad-hoc queries;
- Generate alerts on data anomalies.

Can read data from:

- [Redshift / Redshift Spectrum](docs/database/redshift.md);
- [RDS](docs/database/rds.md);
- [Athena](docs/analytics/athena.md);
- Raw files on [S3](docs/storage/s3.md) or on-premises;
- Databases on [EC2](docs/compute/ecs.md);
- *JIRA* or *SALESFORCE*.

Has some basic data transformations such as renaming columns, setting data types, adding calculated SQL columns, etc.

It is priced per user per month and the subscription can be monthly or annual. It is possible to buy GBs of SPICE store.

**Enterprise Edition** gives *Encryption at rest* and *Microsoft Active Directory* integration.

## SPICE

Usually queries hits Athena, but some of them may take a long time to process. To solve this issue, *SPICE* was created.

*SPICE* stands for Super fast, Parallel, In-memory Calculation Engine and is a *QuickSight* highly available mechanism for storing datasets in-memory in a columnar way, to ensure better performance.

Each user gets 10GB of spice, which is shared with everyone. (Example: 10 users gives a 100GB for them to use.)

However, if it takes more than 30 minutes to import the dataset into *SPICE*, it will still time out.

## Security

Security features include:

- Multi-factor Authentication;
- VPC Connectivity: just add QuickSight's IP range to the database security group;
- Row and column-level security;
- Private VPC Access with *ENI* or AWS Direct Connect.

QuickSight must be authorized to access the underlying data. It is possible to restrict via IAM policies what data in S3 each user can access.

For user access, three options are available:

- IAM
- E-mail Sign-up
- *Active Directory* integration using AD Connector. This is only available for the Enterprise Edition.

## QuickSight Q

A Natural Language Processing Artificial Intelligence powered by Amazon Bedrock.

Users can take full advantage of the controls implemented in Amazon Bedrock to enforce safety, security, and the responsible use of artificial intelligence (AI).

In the Q pane, Amazon Q displays all content that is available based on the context of the task that you are performing. For example, if you're working in an Analysis, you can build a calculation, edit visuals, set up Q&A, or ask questions about your data. If you're working in a Dashboard, you can build a data story, generate an executive summary, or ask questions about the dashboard.

Main highlights are:

- Anomaly Detection using Random Cut Forests.
- Forecasting with ability to ignore outliers. Also uses Random Cut Forests.
- Autonarratives for creating a short text about the data.
- Suggested Insights.

### Particularities with QuickSight and Redshift

By default QuickSight can only access data stored **in the same region**. If QuickSight is running in a region, and Redshift is in another region, there are some workarounds:

- Create a new security group, in the Redshift Cluster, with an inbound rule authorizing access from the IP range of the QuickSight servers in that region.
- With **Enterprise Edition only**: it is possible to solve it, cross-region or cross-account, by putting the services inside a subnet in your VPC using an ENI and then using a **Peering Connection**, an **AWS Transit Gateway**, an **AWS PrivateLink** or use **VPC Sharing** to connect them.

## Anti-Patterns

- Highly formatted reports: colourful, multiple layouts, etc. are not supported.
- ETL, as can do only some limited operations.
