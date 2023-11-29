---
layout: default
title: AWS SAM
parent: Developer Tools
grand_parent: AWS Services
last_modified_date: 2023-11-29
---

# SAM - Serverless Application Model
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

AWS SAM is a framework consisting of two parts: AWS SAM templates (*YAML* files) and the AWS SAM Command Line Interface (AWS SAM CLI). AWS SAM templates provide a short-hand syntax optimized for defining Infrastructure as Code (IaC) for serverless applications that will later be converted into CloudFormation templates.

AWS SAM also supports local development for Lambda, API Gateway, and DynamoDB.

The template must have the key **Transform: 'AWS::Serverless-2016-10-31'**, and the resources are defined as **AWS::Serverless::\[SERVICE\]** where \[SERVICE\] can be *Function* for Lambda function, *Api* for API Gateway, or *SimpleTable* for DybamoDB Table.

For SAM deployment, run:

```bash
# 1. Convert SAM template to CloudFormation Template and organizes the files in the ".aws-sam" folder
aws cloudformation build

# 2. Deploy template, code, and create ClodFormation Change Set.
aws cloudformation deploy --guided

# Note: "aws sam" is a shortcut for "aws cloudformation".
```