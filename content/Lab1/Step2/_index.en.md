---
title: "Step 2. Create a RDS MySQL Database"
chapter: false
weight: 30
---

* Visit the [**Amazon RDS console**](https://console.aws.amazon.com/rds/home?region=us-east-1), then click **Create Database** on top of the page as shown below.

![](/images/lab1-5.png)

* For **Engine options**, select **MySQL**.
* For **Templates**, select **Free tier**.
* In the **Settings** section, for **DB identifier**, enter `wordpress`. 
* Below **Credentials Settings**, enter a random **Master username** and **Master password** (for example: admin123, #12345678aA).

![](/images/lab1-6.png)

![](/images/lab1-7.png)

Now continue in the **Connectivity** section.

* For **Virtual private cloud (VPC)**, select **Vpc / vpc-stack**, which is the VPC that was created in the earlier step through CloudFormation.
* For **VPC security group**, select **Create new**.
* Enter `db-sg` as **New VPC security group name**.

![](/images/lab1-8.png)

* Scroll down and click **Additional configuration**.
* For **Initial database name**, enter `wordpress`.
* Finally, click **Create database**.

![](/images/lab1-9.png)

