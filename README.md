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
### 4. Setup Mysql Server
- First we need to change the default storage engine to innodb and change the default file format to Barracuda, this is a new setting compared to previous versions. You also need to set innodb_file_per_table in order for Barracuda to work properly.
  ```
  sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
  ```
- Scroll down to the [mysqld] section and under Basic Settings add the following line under the last statement.
  ```
  default_storage_engine = innodb
  innodb_file_per_table = 1
  innodb_file_format = Barracuda
  ```
  
### 5. Create Moodle Database
- Log into MySQL and create database for Moodle.
  ```
  $ sudo mysql -u root -p
  ```
- Create a database for moodle, a database user to access it, and also grant full access to this user. Replace moodle_user and moodle_password with your choice of username and password.
  ```
  CREATE DATABASE moodle;
  CREATE USER 'your_username'@'localhost' IDENTIFIED BY 'your_password';
  GRANT ALL ON moodle.* TO 'your_username'@'localhost' IDENTIFIED BY 'your_password' WITH GRANT OPTION;
  ```
- Flush privileges to apply changes.
  ```
  FLUSH PRIVILEGES; 
  EXIT;
  ```
### 6. Download & Install Moodle
- Run the following command to download Moodle package.
  ```
  https://download.moodle.org/stable400/moodle-4.0.3.tgz
  ```
- Run the following command to extract package to NGINX website root folder.
  ```
  $ sudo tar -zxvf moodle-4.0.3.tgz
  $ sudo mv moodle /var/www/html/moodle 
  $ sudo mkdir /var/www/html/moodledata
  ```
- Change the folder permissions.
  ```
  $ sudo chown -R www-data:www-data /var/www/html/moodle/ 
  $ sudo chmod -R 755 /var/www/html/moodle/ 
  $ sudo chown www-data /var/www/html/moodledata
  ```

### 7. Configure NGINX
- Now create a file name **moodle.conf** in the given directory
  ```
  $ sudo vim /etc/nginx/sites-available/moodle.conf
  ```
- Copy paste the following configuration in the above file. Replace example.com below with your domain name.
  ```
  server {
    listen 80;
    listen [::]:80;
    root /var/www/html/moodle;
    index  index.php index.html index.htm;
    server_name  example.com www.example.com;

    location / {
    try_files $uri $uri/ =404;        
    }
 
    location /dataroot/ {
    internal;
    alias /var/www/html/moodledata/;
    }

    location ~ [^/]\.php(/|$) {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

  }
  ```
  
> **Note**: If you are installing locally then replace ```server_name  example.com www.example.com;``` with ```server_name localhost;```

- Enable Moodle site in NGINX by creating a symlink as shown below.
  ```
  $ sudo ln -s /etc/nginx/sites-available/moodle /etc/nginx/sites-enabled/
  ```
- Restart NGINX Server to apply changes.
  ```
  $ sudo service nginx restart
  ```
