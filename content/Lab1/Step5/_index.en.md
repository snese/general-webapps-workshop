---
title: "Step 5. Set up the WordPress Environment"
chapter: false
weight: 30

---

* Visit [**EC2 console**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:sort=instanceId), 
* Click the **Edit** button under **Name** column, enter `Public`  and click **Save**
* Select the **Public** instance and click **Connect**, 
* In **Connect to instance** page, click **SSH client** on menu and you can find the **connection command** in **Example**.

![](/images/lab1-15.png)

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
/** to allow https in WordPress, will be used in lab 2*/
$_SERVER['HTTPS'] = 'on';
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
