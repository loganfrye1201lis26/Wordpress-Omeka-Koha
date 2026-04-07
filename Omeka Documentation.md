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
