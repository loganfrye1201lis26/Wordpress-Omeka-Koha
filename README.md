# Wordpress-Omeka-Koha

# WordPress Installation
## Overview
In this project, I installed WordPress on a virtual machine running a LAMP stack (Linux, Apache, MySQL, PHP). This builds on previous assignments where I created a bare-bones OPAC and configured Apache and MySQL manually. The goal was to understand how dynamic web applications interact with databases and web servers in a real-world environment.

## Step 1: Prepare the Environment
Before installing WordPress, I verified that Apache, MySQL, and PHP were installed and functioning correctly.
[sudo systemctl status apache2]
[php -v]
[mysql --version]

## Step 2: Install Required PHP Modules
[sudo apt update]
[sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl -y]
[sudo systemctl restart apache2]

## Step 3: Download WordPress
[cd /var/www/html]
[sudo wget https://wordpress.org/latest.zip]
Since the [unzip] utility was not installed, I installed it: [sudo apt install unzip -y]
Then extracted the files: [sudo unzip latest.zip]
This created a [/var/www/html/wordpress] directory

## Step 4: Configure the Database
I created a new MySQL database and user specifically for WordPress:
[sudo mysql
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'Password123!';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
EXIT;]
**This separates WordPress data from other applications like the OPAC.**

## Step 5: Configure WordPress
[cd /var/www/html/wordpress 
sudo cp wp-config-sample.php wp-config.php]
This file allows WordPress to connect to the MySQL database.

## Step 6: Set Permissions
[sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress]

## Step 7: Complete Installation via Browser
I accessed WordPress in a web browser using [http://35.184.73.236/wordpress/] (http://35.184.73.236/wordpress/wp-admin/)

## Complications & Reflection
One of the main issues I encountered was that the WordPress directory did not initially exist because the zip file had not been extracted. Additionally, the unzip utility was missing, which prevented extraction. Installing unzip resolved this issue.
Another issue involved MySQL errors when attempting to create users that already existed. Instead of recreating users, I used ALTER USER and GRANT to fix permissions.
I also had to verify that Apache was running and that I was using the correct external IP address. Testing with curl http://localhost helped confirm that the server was functioning properly.

This assignment reinforced how all components of a LAMP stack work together. While WordPress simplifies website creation, the installation process highlights the importance of server configuration, file permissions, and database management.
Compared to the bare-bones OPAC, WordPress abstracts much of the backend logic, but requires correct setup to function. This experience helped me better understand how content management systems operate on top of web infrastructure and how small configuration issues (such as missing directories or incorrect permissions) can prevent the entire system from working. Overall, this project helped bridge the gap between manually coded systems and fully developed web applications used in real library environments.
