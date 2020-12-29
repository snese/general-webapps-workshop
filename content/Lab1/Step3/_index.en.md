---
title: "Step 3. Launch a EC2 Instance"
chapter: false
weight: 30
---




### 3. Launch a EC2 Instance

* Visit **EC2 console** and click Launch Instances
* In **Choose an Amazon Machine Image (AMI)** page, select **Amazon Linux 2 AMI (HVM), SSD Volume Type**,
* In **Choose an Instance Type** page, select **t2.micro**
* Click **Next: Configure Instance Details** button

![](/image/lab1-10.png)

![](/image/lab1-11.png)

* In **Configure Instance Details** page
* For **Network**, select **Vpc / vpc-stack**
* For **Subnet**, select **PublicSubnet0**
* Click **Next: Add Storage** → **Next: Add Tags**

![](/image/lab1-12.png)

* For **Assign a security group**, select **Create a new Security Group**
* For **Security group name**, enter `public-instance-sg `
* In the pre-created rule, 
* For **Source**, select **My IP**, your IP will be auto-detected, and click **Review and Launch** → **Launch**

![](/image/lab1-13.png)

* Select **Create a new key pair**, Enter the **Key pair name** and click **Download key pair**
* Finally, **Launch** the instance

![](/image/lab1-14.png)

