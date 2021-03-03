---
title: "Step 1. Create a VPC CloudFormation Stack"
chapter: false
weight: 30
---

* Copy [**sample code**](/Lab1/Step1/template/new-vpc.template) and paste in a new file named `new-vpc.template`

* Visit [**CloudFormation console**](https://console.aws.amazon.com/cloudformation/home?region=us-east-1), upload the `new-vpc.template` just created, and Click **Next**


![](/images/lab1-0.png)

* For **Stack name**, enter `vpc-stack` , For **Number of Availability Zones** select **2**, 
* For **Availability Zones**, choose **us-east-1a** and **us-east-1b**, leave other setting as default value and click **Next**
* Click **Next** in **Configure stack options** stage. 
* On the **Review** page, review and confirm the settings. 
* Check the box acknowledging that the template will create IAM resources and Click **Create Stack**

![](/images/lab1-2.png)

![](/images/lab1-3.png)

* After 3-5 minute, visit your **CloudFormation stacks** page, when the status of vpc-stack shows **CREAT_COMPLETE**, click **Outputs** in menu, you will see several resources are created.

![](/images/lab1-4.png)

