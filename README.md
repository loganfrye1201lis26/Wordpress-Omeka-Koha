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

# Omeka Installation
## Overview
In this project, I installed Omeka Classic on a virtual machine running a LAMP stack (Linux, Apache, MySQL, PHP). This builds on previous assignments where I installed WordPress and created a bare-bones OPAC. The goal of this installation was to understand how a digital collections platform operates and how it connects to a database and web server environment.

## Step 1: Create Database User
I created a new MySQL database and user specifically for Omeka. I made sure not to reuse any credentials from the WordPress installation.
[sudo mysql
CREATE DATABASE omeka;
CREATE USER 'omekauser'@'localhost' IDENTIFIED BY 'Password123!';
GRANT ALL PRIVILEGES ON omeka.* TO 'omekauser'@'localhost';
FLUSH PRIVILEGES; 
EXIT;]

## Step 2: Download Omeka
[cd /var/www/html
sudo wget https://github.com/omeka/Omeka/releases/latest/download/omeka-3.2.zip]

## Step 3: Extract Files
[sudo unzip omeka-3.2.zip
ls]
This created a directory with a version number in the name.

## Step 4: Rename Directory
To simplify the URL and file structure, I renamed the extracted directory:
[sudo mv omeka-3.2 omeka]

## Step 5: Configure Database Connection
I edited the [db.ini] file to connect Omeka to the MySQL databse.
[cd /var/www/html/omeka
sudo nano db.ini]

Updated values:
[host = "localhost"
username = "omekauser"
password = "Password123!"
dbname = "omeka"]

## Step 6: Set Permissions
[sudo chown -R www-data:www-data /var/www/html/omeka
sudo chmod -R g+w *]
 Restart Apache
[sudo systemctl restart apache2]

## Step 7: Complete Installation via Browser
I accessed the Omeka setup page: http://35.184.73.236/omeka/

## Challenges & Reflection
The installation process was smoother compared to WordPress because I had already worked through similar steps. However, I had to ensure that the database credentials in [db.ini] were correct and that I did not reuse existing databases or users. I also had to verify that the unzip utility was installed before extracting the files. File permissions were another important factor, as incorrect permissions can prevent Apache from accessing the application properly.

This assignment reinforced my understanding of how web applications are deployed in a LAMP environment. Omeka differs from WordPress in that it is designed specifically for digital collections and archival materials rather than general website creation. Despite this difference, the installation process highlighted the same core concepts: database configuration, file management, and server permissions. Completing this installation independently increased my confidence in deploying and troubleshooting web-based library systems.
