+++
title = "Lab1"
chapter = false
weight = 30
+++



### 1. Create a VPC CloudFormation Stack

* Visit [**CloudFormation console**](https://console.aws.amazon.com/cloudformation/home?region=us-east-1), paste **Amazon S3 URL** below, and Click **Next**
    `https://wp-workshop-template.s3.amazonaws.com/newvpc.template`

![](https://i.imgur.com/tw0kfyC.png)

* For **Stack name**, enter `vpc-stack` , For **Number of Availability Zones** select **2**, 
* For **Availability Zones**, choose **us-east-1a** and **us-east-1b**, leave other setting as default value and click **Next**
* Click **Next** in **Configure stack options** stage. 
* On the **Review** page, review and confirm the settings. 
* Check the box acknowledging that the template will create IAM resources and Click **Create Stack**

![](https://i.imgur.com/tWmHJGg.png)

![](https://i.imgur.com/hQIotgz.png)

* After 3-5 minute, visit your **CloudFormation stacks** page, when the status of vpc-stack shows **CREAT_COMPLETE**, click **Outputs** in menu, you will see several resources are created.

![](https://i.imgur.com/fh642o0.png)

### 2. Create a RDS MySQL Database

* Visit [**RDS console**](https://console.aws.amazon.com/rds/home?region=us-east-1), click **Create Database**

![](https://i.imgur.com/Ud8Xy4y.png)

* For **Engine options**, select **MySQL**
* For **Templates**, select **Free tier**, 
* In **Settings** section, 
* For **DB identifier**, enter `wordpress`, 
* For **Credentials Settings**, enter your **Master username** and  **Master password** (for example: #12345678aA)

![](https://i.imgur.com/wwKhUUz.png)

![](https://i.imgur.com/s2SZrzq.png)

* In **Connectivity** section, 
* For **Virtual private cloud (VPC)**, select **Vpc / vpc-stack** created in last step
* Click **Additional connectivity configuration** to show more configuration
* For **Public access**, select **No**
* For **VPC security group**, select **Create new** and enter `db-sg` in **New VPC security group name**

![](https://i.imgur.com/3O70TNI.png)

* Scroll down and click **Additional configuration**, 
* For **Initial database name**, enter `wordpress`,  
* Finally, click **Create database**

![](https://i.imgur.com/xkFZZr4.png)

### 3. Launch a EC2 Instance

* Visit **EC2 console** and click Launch Instances
* In **Choose an Amazon Machine Image (AMI)** page, select **Amazon Linux 2 AMI (HVM), SSD Volume Type**,
* In **Choose an Instance Type** page, select **t2.micro**
* Click **Next: Configure Instance Details** button

![](https://i.imgur.com/Nc3GX30.png)

![](https://i.imgur.com/E5O3WJA.png)

* In **Configure Instance Details** page
* For **Network**, select **Vpc / vpc-stack**
* For **Subnet**, select **PublicSubnet0**
* Click **Next: Add Storage** → **Next: Add Tags**

![](https://i.imgur.com/QDF5k9f.png)

* For **Assign a security group**, select **Create a new Security Group**
* For **Security group name**, enter `public-instance-sg `
* In the pre-created rule, 
* For **Source**, select **My IP**, your IP will be auto-detected, and click **Review and Launch** → **Launch**

![](https://i.imgur.com/8W4MKr7.png)

* Select **Create a new key pair**, Enter the **Key pair name** and click **Download key pair**
* Finally, **Launch** the instance

![](https://i.imgur.com/WIGJ0VK.png)

### 4. Modify Security Groups of RDS and EC2 instance

* Visit [**Security Groups**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#SecurityGroups:) page, select `public-instance-sg `, 
* Click **Edit inbound rules** button
* Click **Add rule**, For **Type**, select **MYSQL/Aurora**, 
* For **Source**, select custom and find the `db-sg` , and click **Save rules**
* Click **Add rule**, For **Type**, select **HTTP**, 
* For **Source**, select **My IP** , and click **Save rules**
* Visit [**Security Groups**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#SecurityGroups:) page, select `db-sg` , 
* Click **Edit inbound rules** button
* Click **Add rule**, For **Type**, select **MYSQL/Aurora**, 
* For **Source**, select custom and find the `wordpress-instance-sg` , and click **Save rules**



### 5. Set up the WordPress Environment

* Visit [**EC2 console**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:sort=instanceId), 
* Click the **Edit** button under **Name** column, enter `Public`  and click **Save**
* Select the **Public** instance and click **Connect**, 
* In **Connect to instance** page, click **SSH client** on menu and you can find the **connection command** in **Example**.

![](https://i.imgur.com/6bGhVJ1.png)

**Windows User:** follow the step in [this deck](https://github.com/snese/general-webapps-workshop/blob/main/content/Reference/putty_setup.pdf) to connect EC2 instance by [**Putty**](https://www.putty.org/)

**MacOS User:** open a **terminal** on your computer, navigate to the folder where your **.pem key file** is stored, and paste the connection command.

* After connection successes, run the following command to install Apache and MySQL on your instance

```
sudo yum install -y httpd
sudo yum install -y mysql
```

* Set the environment variable of MySQL in your computer, replace `<your-endpoint>` to the endpoint which can be found in [**RDS console**](https://console.aws.amazon.com/rds/home?region=us-east-1#databases:)→ your database

```
export MYSQL_HOST=<your-endpoint>
```

* Replace your own user name and Password and run the following command to connect to wordpress database

```
mysql --user=<your-username> --password=<your-password> wordpress
```

* Create a new DB for WordPress and grant the permission 

```
CREATE USER 'wordpress' IDENTIFIED BY 'wordpress-pass';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpress;
FLUSH PRIVILEGES;
Exit
```

* Then, download the WordPress module and unzip it

```
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
```

* Move into WordPress folder and backup the default config file

```
cd wordpress
cp wp-config-sample.php wp-config.php
```

* After that use nano to edit the **wp-config.php** file

```
nano wp-config.php
```

* Modify the following script into the correct value: 

● **DB_NAME:** `'wordpress'`
● **DB_USER:** `'wordpress'`
● **DB_PASSWORD:** `'wordpress-pass'`
● **DB_HOST:** `your RDS endpoint `

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */

define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );
```

* Visit [**this link**](https://api.wordpress.org/secret-key/1.1/salt/) copy the content in the page and replace the following script into new value 

```
/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );

```



* Also, paste the script below. After modifying the right value in configuration file, use **CTRL + O** to save file, **CTRL + X **to quit** nano editor**

```
/** to allow 'W3TC' plugin write the configuration data into DB */
define( 'W3TC_CONFIG_DATABASE', true );
```

* Run the following command to deploy WordPress on your computer: 

```
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install php-xml 
cd /home/ec2-user
sudo cp -r wordpress/* /var/www/html/
sudo chown -R apache:apache /var/www/html
```

* Finally, start hosting the Apache web server

```
sudo service httpd start
sudo systemctl restart php-fpm
```

### 6. Visit your website and configure the WordPress setting

* Visit [**IAM console**](https://console.aws.amazon.com/iam/home?region=us-east-1#/home)
* Click **Users** on the left menu, then click **Add user** 
* Enter`wordpress-user` in **username**
* For **Select AWS access type**, select **Programmatic access** and click **Next: Permissions**
* For **Set permissions**, select **Attach existing policies directly** Search and attach **CloudFrontFullAccess** and **AmaoznS3FullAccess** 
* Click **Next: Tags** → **Next: Review** → **Create user** 
* Click **Download .csv** to store your Access/Secret key

![](https://i.imgur.com/TdQOjBC.png)

![](https://i.imgur.com/nnkIFkj.png)

![](https://i.imgur.com/AWvREXu.png)

![](https://i.imgur.com/fnhO0kA.png)


* Open another tab/window and visit [**EC2 console**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:sort=instanceId)
* Select the EC2 instance, and find the **Public IPv4 DNS** in **Details** below and paste it on your browser, then you will see the setup page of WordPress.

![](https://i.imgur.com/BCP7WUk.png)

* In setup page, enter your own value in the **Site Title**, **Username**, **Password** and **Your Email**, then click **Install WordPress**
    
![](https://i.imgur.com/lDNIOMy.png)

* After few seconds, it will be redirected to Login page, 
* Enter your **username** and **password**, and click **Login** button, you will see the **admin** page,
* In admin page, click **Plugins** on the left menu, 
* Click **Add New**, search `w3 total cache` and click **Install Now → Activate**,
* Click **Performance/General setting** on the left menu, 
* In **CDN** section, for **CDN type**, select **Origin Push:S3**, 
* For **CDN**, make sure it is **Enable** and Click **Save all Settings**

![](https://i.imgur.com/AeH70D8.png)

![](https://i.imgur.com/yvYxvbb.png)

* Click **Performance/CDN** on the left menu, scroll down to find **Configuration: Objects** section 
* Copy and paste the **Access key ID** and **Secret key** from IAM user creation page
* For **Bucket**, enter `wp-workshop-<custom name>` and click **Create as new bucket**
* Click **Test S3 upload** to ensure connection succeeded and click **Save settings & Purge Caches**
* Click the **Page/All pages** on the left menu, click **Simple page** to edit it click “+” icon on the top left and insert a sample image, then click **Update** to confirm the change. [download sample image](https://d1.awsstatic.com/logos/aws-logo-lockups/poweredbyaws/PB_AWS_logo_RGB_REV_SQ.8c88ac215fe4e441dc42865dd6962ed4f444a90d.png)
* Click **Performance/CDN** the left menu and click **export the media library, wp-includes, theme files**  
* Click **custom files → start** button to export file from EC2 to S3 bucket

![](https://i.imgur.com/3xifcrA.png)

![](https://i.imgur.com/CxNOyQS.png)

* Now, you can view your sample page in `<your ec2 domain>/index.php/sample-page/`
    
![](https://i.imgur.com/KWT0P0r.png)
