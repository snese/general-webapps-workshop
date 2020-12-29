---
title: "Lab 2 - Build a website with High Availability"
chapter: false
weight: 30
tags:
  - ALB
  - Auto Scaling Group
  - RDS MySQL
  - CloudFront
  - EC2
---

In Lab 2, we first create an custom AMI using the EC2 instance built in Lab 1, then create an application load balancer, attach the  [Auto Scaling Group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) which launches EC2 instances in private subnet. Finally, we create an [CloudFront](https://aws.amazon.com/cloudfront) distribution as CDN and change the configuration of WordPress. 