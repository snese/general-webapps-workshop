---
title: "Lab2"
chapter: false
weight: 30
tags:
  - ALB
  - Auto Scaling Group
  - RDS MySQL
  - CloudFront
  - EC2
---


### 1. Create an Application Load Balancer

* Visit EC2 instance page, select the public WordPress instance created in Lab1
* Click **Actions** → **Image and templates** → **Create image**
* Enter the **Image name** and click **Create image**

![](https://i.imgur.com/lmMDpCH.png)

* Visit **EC2/Load Balancing/Load Balancers**
* Click **Create Load Balalncer**

![](https://i.imgur.com/OAgmUPI.png)

* In **Step 1: Select load balancer type**, find **Application Load Balancer** and click **Create**

![](https://i.imgur.com/O4EsZ8m.png)

* In **Basic Configuration** section, enter the name `wordpress-alb`
* In **Availability Zones**, for **VPC**, choose **Vpc / vpc-stack** created by CloudFormation
* For **Availability Zones**, select **us-east-1a/PublicSubnet0** and **us-east-1b/PublicSubnet1**

![](https://i.imgur.com/Ht6Jp0W.png)

* In **Step 2: Configure Security Settings**, click **Next**
* In **Step 3: Configure Security Groups**, for **Assign a security group**, choose **Create a new security group**
* For **Security group name**, enter `alb-sg` and click **Next Configure Routing**

![](https://i.imgur.com/0hG9G1S.png)

* In **Step 4: Configure Routing**
* For **Target Group**, select **New target group**
* For **Name**, enter `wordpress-tg`
* For **Target type**, select **Instance** and click **Next: Register Targets**

![](https://i.imgur.com/vGqR8V9.png)

* In **Register Targets** stage, click **Next: Review**
* In **Review** stage, click **Create**

### 2. Create an Auto-Scaling Group

* Visit **EC2/Network & Security/Security Groups**
    Click **Create security group**

![](https://i.imgur.com/l8pJvGb.png)

* For **Security group name**, enter `asg-sg`
* For **Description**, enter `asg-sg`
* In **Inbound rules** section
* Click **Add rule**
* For **Type**, select **HTTP**
* For **Source**, select **Custom** and find **alb-sg**
* Click **Add rule again**
* For **Type**, select **MYSQL/AURORA**
* For **Source**, select **Custom** and find **db-sg**
* Click **Create security group**

![](https://i.imgur.com/kuYzJTo.png)

* Visit **EC2/Network & Security/Security Groups**
* Find **db-sg** and click its **Security group ID**
* Click **Edit inbound rules**
* Click **Add rule**
* For **Type**, select **MYSQL/AURORA**
* For **Source**, select **Custom** and find **asg-sg**
* Click **Save Rules**
* Visit **EC2/Auto Scaling/Launch Configurations** 
* Click **Create Launch configuration**
* For **name**, enter `wordpress`
* For **AMI**, choose AMI created in last step
* For **Instance type**, search and select **t2.micro**

![](https://i.imgur.com/Sur2IKo.png)

* In **Additional configuration** section, click **Advanced details**
* For **User data**, select **As text** and enter the script below
```
#!/bin/bash
yum update -y
sudo service httpd restart
```

![](https://i.imgur.com/7rdswGL.png)

* For **Security group**,select **Select an existing security group** and select **asg-sg** just created
* For **Key pair options**, select **Choose an existing key pair**
* For **Existing key pair**, select the key created in Lab 1
* Finally, click **Create launch configuration**

![](https://i.imgur.com/KTfZwpm.png)

* Visit **EC2/Auto Scaling/Auto Scaling Groups**
* Click **Create an Auto Scaling Group**
* For **Auto Scaling group name**, enter` wordpress-sg `
* For **Launch template** section, click **Switch to launch configuration** and select the launch configuration created in last step and click **Next** 

![](https://i.imgur.com/K9xdcBb.png)

* In **Configure setting** stage,
* For **Vpc**, select **Vpc / vpc-stack**, created by CloudFormation
* For **Subnets**, select **WebSubnet0 / vpc-stack** and **WebSubnet1 / vpc-stack**, then click **Next**
* In **Configure advanced options** stage
* For **Load balancing**, select **Attach to an existing load balancer**
* For **Existing load balancer target groups**, select **alb-tg**

![](https://i.imgur.com/9eA6SmS.png)

* In **Configure group size and scaling policies** stage
* In **Group size - optional** Section
* For **Desired capacity, Minimum capacity, Maximum capacity**, enter **2,2,3**  then click **Next**
* In **Add notifications** and **Add tags** sections, click **Next**

![](https://i.imgur.com/ImuxvX5.png)

![](https://i.imgur.com/Ax28s5D.png)

* In **Review** sections, click Create **Auto Scaling group** 

### 3. Create a CloudFront Distribution


#### 3a. Create distribution via cli

[GitHub Repo](https://github.com/hhh2012aa/wordpress-cloudfront-distrubution-config-wizard)

* Open a Cloud9 IDE, and run following scripts in terminal

```
git clone https://github.com/hhh2012aa/wordpress-cloudfront-distrubution-config-wizard
cd wordpress-cloudfront-distrubution-config-wizard/
python create-config.py
```

```
# Create CloudFront distribution using configuration file
aws cloudfront create-distribution --distribution-config file://my-create-CloudFront.json
```

#### 3b. Create distribution manually

* Visit CloudFront console, and click **Create distributions**, choose **Web** for delivery method

![](https://i.imgur.com/oelcoPd.png)


* For **Origin Domain Name**, select **wordpress-alb**

![](https://i.imgur.com/015OOCn.png)

* In **Default Cache Behavior Settings**
* For **Origin Protocol Policy**, select **HTTP and HTTPS**
* For **Allowed HTTP Methods**, select **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**
* For **Cached HTTP Methods**, select **GET, HEAD, OPTIONS**
* For **Cache and origin request settings**, select **Use legacy cache settings**
* For **Cache Based on Selected Request Headers, select Whitelist**
* For **Whitelist Headers**, search and add **Host** and **Origin**
* For **Object Caching**, select **Customize**
* For **Minimum TTL**, enter `0`
* For **Maximum TTL**, enter `31536000`
* For **Default TTL**, enter `300`
* For **Forward Cookies**, select **All** 

![](https://i.imgur.com/MQh8D34.png)

* For **Query String Forwarding and Caching**, select **Forward all, cache based on all**
* For **Smooth Streaming**, select **No**
* For **Restrict Viewer Access**, select **No**
* For **Compress Objects Automatically**, select **Yes**
* Finally, **Create Distribution**

![](https://i.imgur.com/C1nLtKm.png)

* Visit CloudFront Distribution page
* Click the **distribution ID** created in last step
* Click **Origins and Origin Groups **tab, and click **Create Origin**
* For **Origin Domain Name**, select **S3 bucket** created in Lab 1 and Click **Create**

![](https://i.imgur.com/eTomuZf.png)

* Next, move to **Behavior** in your Distribution and click **Create Behavior**, follow the table below to create 4 new behaviors:



|Path Pattern	|/wp-includes/*	|/wp-content/*	|
|---	|---	|---	|
|Origin Domain Name	|<your S3 bucket>	|<your S3 bucket>	|
|Viewer Protocol Policy	|HTTP and HTTPS	|HTTP and HTTPS	|
|Allowed HTTP Methods	|GET, HEAD, OPTIONS	|GET, HEAD, OPTIONS	|
|Cached HTTP Methods	|GET, HEAD, OPTIONS	|GET, HEAD, OPTIONS	|
|Cache and origin request settings	|Use legacy cache settings	|Use legacy cache settings	|
|Cache Based on Selected Request Headers	|Whitelist	|Whitelist	|
|Whitelist Headers	|Origin, Access-Control-Request-Headers,Access-Control-Request-Method|Origin, Access-Control-Request-Headers,Access-Control-Request-Method	|
|Object Caching	|Customize	|Customize	|
|Min/Max/Default TTL	|0/604800/86400	|0/604800/86400	|
|Forward Cookies	|None	|None	|
|Query String Forwarding and Caching	|None	|None	|
|Smooth Streaming	|No	|No	|
|Restrict Viewer Access	|No	|No	|
|Compress Objects Automatically	|Yes	|Yes	|

### 4. Modify the WordPress Settings

* Find the Domain name of ALB Created in Step 1, paste it on browser to visit your WordPress page, scroll down and click **Log in** to enter the admin page

![](https://i.imgur.com/Jzb38hy.png)

* In admin page, click **Performance/General Settings** on the left menu
* Scroll down to find the **CDN** section
* For **CDN type**, select **Amazon CloudFront Over S3**
* Click **Save Settings and Purge Caches**

![](https://i.imgur.com/VF0nxkd.png)

* Click **Performance/CDN** on the left menu, scroll down and find **Configuration:Objects** section
* For **Access key ID** and **Secret key**, paste your IAM user credentials
* For **Bucket**, enter your S3 bucket name created in Lab 1
* For **Replace site's hostname with**, enter the CloudFront Domain created in last step and click **Save Settings and Purge Caches**

![](https://i.imgur.com/cCd3tsv.png)

* Click **Setting/General** on the left menu
* For **WordPress Address (URL)** and **Site Address (URL)**, enter your CloudFront domain and click **Save Changes**

![](https://i.imgur.com/mEOTsq8.png)
