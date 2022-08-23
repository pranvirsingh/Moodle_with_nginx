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
  $ sudo systemctl stop nginx.service 
$ sudo systemctl start nginx.service 
$ sudo systemctl enable nginx.service
```
  
- Run the following command to secure MariaDB installation.
  > $ sudo mysql_secure_installation
  
### 3. Install PHP-FPM & Related modules
- Run the following command to add PHP repository, install it along with other related modules.
  > 
