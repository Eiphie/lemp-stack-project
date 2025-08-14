# LEMP stack
LEMP is an acronym for Linux, Nginx, MySQL, PHP or Python or Perl, which is a set of technology framework and tools used to develop a software product.

## Project Overview
This is a simple project, you will implement similar stack but with an alternative server called `nginx` which is widely used by many websites in the internet.

## Project SetUp
This project assumes that you have an active AWS account, and you have launched EC2 instance of t2.micro family with ubuntu server 20.04 LTS.

### Installations:
- Install Nginx
```
sudo apt update
sudo apt install nginx
```
Verify Nginix installation
```
sudo systemctl status nginx
```
Confirm that server is running and can be accessed locally and from the internet
```
curl http://localhost:80
```

```
http://<Public-IP-Address>:80
```
- Install MySQL
```
sudo apt install mysql-server
```
Verify MySQL installation
```
sudo mysql
```
- Install PHP
```
sudo apt install php-fpm php-mysql
```
- Configure NginX to use PHP Processor
Create web directory for your_domain
```
sudo mkdir /var/www/ProjectLEMP
```
Assign ownership of the directory to USER
```
sudo chown -R $USER:$USER /var/www/ProjectLEMP
```
Create new configuration file in NginX `sites-avalable`
```
sudo vim /etc/nginx/sites-available/ProjectLEMP
```
Create new blank file
```
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }
```
Create an `index.html` file
```
sudo sh -c 'echo "Hello LEMP from hostname $MYHOST with public IP $MYIP > /var/www/projectLEMP/index.html'
```
- Test PHP with NginX
Open a new file called `info.php`
```
sudo vim /var/www/ProjectLEMP/info.php
```
Use PHP code to return information about browser
```
<?php
phpinfo();
```
Access page in your web browser
```
http://`server_domain_or_IP`/info.php
```
- Using PHP to retrieve data from MySQL
Create a new database after connecting to MySQL.
```
mysql> CREATE DATABASE `example_database`;
```
Create a new user and give them full database access.
```
