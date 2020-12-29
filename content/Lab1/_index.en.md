---
title: "Lab 1 - Host a sigle-instance website"
chapter: false
weight: 30
tags:
  - VPC
  - S3
  - CloudFormation
  - RDS MySQL
  - EC2
  - Security Group
  - IAM
---

In Lab 1, we will first deploy the [VPC](https://aws.amazon.com/vpc) using [CloudFormation](https://aws.amazon.com/cloudformation/) template, then create  an database on [Amazon RDS](https://aws.amazon.com/rds/), also launch an [EC2 instance](https://aws.amazon.com/ec2). After that, we will set up the connection between RDS and EC2, then install the WordPress website on the instance. Finally, we will start hosting a simple WordPress website on EC2 instance and export the static assets into S3 bucket.