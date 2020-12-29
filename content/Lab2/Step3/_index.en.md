---
title: "Step 3. Create a CloudFront Distribution"
chapter: false
weight: 30

--- 

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

![](/image/lab2-16.png)


* For **Origin Domain Name**, select **wordpress-alb**

![](/image/lab2-17.png)

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

![](/image/lab2-18.png)

* For **Query String Forwarding and Caching**, select **Forward all, cache based on all**
* For **Smooth Streaming**, select **No**
* For **Restrict Viewer Access**, select **No**
* For **Compress Objects Automatically**, select **Yes**
* Finally, **Create Distribution**

![](/image/lab2-19.png)

* Visit CloudFront Distribution page
* Click the **distribution ID** created in last step
* Click **Origins and Origin Groups **tab, and click **Create Origin**
* For **Origin Domain Name**, select **S3 bucket** created in Lab 1 and Click **Create**

![](/image/lab2-20.png)

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
