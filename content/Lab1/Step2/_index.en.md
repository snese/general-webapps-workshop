---
title: "Step 2. Create a RDS MySQL Database"
chapter: false
weight: 30
---

* Visit [**RDS console**](https://console.aws.amazon.com/rds/home?region=us-east-1), click **Create Database**

![](/images/lab1-5.png)

* For **Engine options**, select **MySQL**
* For **Templates**, select **Free tier**, 
* In **Settings** section, 
* For **DB identifier**, enter `wordpress`, 
* For **Credentials Settings**, enter your **Master username** and  **Master password** (for example: #12345678aA)

![](/images/lab1-6.png)

![](/images/lab1-7.png)

* In **Connectivity** section, 
* For **Virtual private cloud (VPC)**, select **Vpc / vpc-stack** created in last step
* Click **Additional connectivity configuration** to show more configuration
* For **Public access**, select **No**
* For **VPC security group**, select **Create new** and enter `db-sg` in **New VPC security group name**

![](/images/lab1-8.png)

* Scroll down and click **Additional configuration**, 
* For **Initial database name**, enter `wordpress`,  
* Finally, click **Create database**

![](/images/lab1-9.png)

