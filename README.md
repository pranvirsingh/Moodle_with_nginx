# **Moodle_with_nginx**

## Prerequisites:
- Moodle version 4.0.1+
- php version 7+ (In my case i use php7.4)
- Mariadb or MySql(any one)
- vim editor

## STEPS

### 1. Install **Nginx**
- Open terminal and run the following commands to update system and install NGINX.
  ``` 
  $ sudo apt-get update
  $ sudo apt-get install nginx 
  ```
- Run the following commands to enable NGINX to autostart during reboot, and also start it now.
  ```
  $ sudo systemctl stop nginx.service 
  $ sudo systemctl start nginx.service 
  $ sudo systemctl enable nginx.service
  ```

### 2. Install MariaDB/MySQL
- Run the following commands to install MariaDB database for Moodle. You may also use MySQL instead.
  ```
  sudo apt install mariadb-server-10.6 mariadb-client-10.6
  ```
  
- Like NGINX, we will run the following commands to enable MariaDB to autostart during reboot, and also start now.
  ```
  $ sudo systemctl stop mariadb.service 
  $ sudo systemctl start mariadb.service 
  $ sudo systemctl enable mariadb.service
  ```
  
- Run the following command to secure MariaDB installation.
  ```
  $ sudo mysql_secure_installation
  ```
  
### 3. Install PHP-FPM & Related modules
- Run the following command to add PHP repository, install it along with other related modules.
  ```
  $ sudo apt-get install software-properties-common
  $ sudo add-apt-repository ppa:ondrej/php
  $ sudo apt update
  $ sudo apt install graphviz aspell ghostscript clamav php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring php7.4-fpm php7.4-cgi
  ```
  
### 4. Create Moodle Database
- Log into MySQL and create database for Moodle.
  ```
  $ sudo mysql -u root -p
  ```
- Create a database for moodle, a database user to access it, and also grant full access to this user. Replace moodle_user and moodle_password with your choice of username and password.
  ```
  CREATE DATABASE moodle;
  CREATE USER 'moodle_user'@'localhost' IDENTIFIED BY 'moodle_password';
  GRANT ALL ON moodle.* TO 'moodle_user'@'localhost' IDENTIFIED BY 'moodle_password' WITH GRANT OPTION;
  ```
- Flush privileges to apply changes.
  ```
  FLUSH PRIVILEGES; 
  EXIT;
  ```
### 5. Download & Install Moodle
- Run the following command to download Moodle package.
  ```
  https://download.moodle.org/stable400/moodle-4.0.3.tgz
  ```
