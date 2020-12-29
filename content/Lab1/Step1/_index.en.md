---
title: "Step1. Create a VPC CloudFormation Stack"
chapter: false
weight: 30
---


### 1. Create a VPC CloudFormation Stack

* Visit [**CloudFormation console**](https://console.aws.amazon.com/cloudformation/home?region=us-east-1), paste **Amazon S3 URL** below, and Click **Next**
    `https://wp-workshop-template.s3.amazonaws.com/newvpc.template`

![](/assets/lab1-1.png)

* For **Stack name**, enter `vpc-stack` , For **Number of Availability Zones** select **2**, 
* For **Availability Zones**, choose **us-east-1a** and **us-east-1b**, leave other setting as default value and click **Next**
* Click **Next** in **Configure stack options** stage. 
* On the **Review** page, review and confirm the settings. 
* Check the box acknowledging that the template will create IAM resources and Click **Create Stack**

![](/assets/lab1-2.png)

![](/assets/lab1-3.png)

* After 3-5 minute, visit your **CloudFormation stacks** page, when the status of vpc-stack shows **CREAT_COMPLETE**, click **Outputs** in menu, you will see several resources are created.

![](/assets/lab1-4.png)

