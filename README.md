# Ubuntu 20 Webserver Php 7.4

## Installation Guide

Update and Upgrade

```sh
sudo apt-get update && sudo apt-get upgrade
```

Install the Apache Web Server on Ubuntu 20.04 [link](https://linuxhint.com/install_apache_web_server_ubuntu/)

```sh
sudo apt install apache2
apache2 -version
sudo ufw app list
```

Set AllowOverride all /var/www/ directory

```sh
sudo nano /etc/apache2/apache2.conf
```

change "AllowOverride None" to "AllowOverride All"

```
<Directory /var/www/>
	Options Indexes FollowSymLinks
		AllowOverride All
	Require all granted
</Directory>
```
Install PHP 7.4 on Ubuntu 20.04 [link](https://computingforgeeks.com/how-to-install-php-on-ubuntu/)

```sh
sudo apt-get install php php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```

PHP Configuration

```sh
sudo nano /etc/php/7.4/apache2/php.ini
``` 

hint:
search text: "ctrl+w" 

```
max_execution_time=120
max_input_time=120
max_input_vars=50000    remove;
memory_limit=256M
post_max_size=64M
upload_max_filesize=128M
``` 

Restart Apache2

```sh
sudo systemctl restart apache2
sudo systemctl status apache2
```
hint:
keypress "q" exit command for "systemctl status apache2"

Install MariaDB on Ubuntu 20.04

```sh
sudo apt-get install mariadb-server
sudo systemctl start mariadb.service
sudo mysql_secure_installation
```

>Change the root password? [Y]<br>
Remove anonymous users? [Y]<br>
Disallow root login remotely? [Y]<br>
Remove test databse ... [Y]<br>
Reload privilege tables now ? [Y]<br>

Restart Mariadb

```sh
sudo systemctl restart mariadb.service
sudo systemctl status mariadb
```

Install phpMyAdmin

```sh
sudo apt-get install phpmyadmin
```
>[*] apache2 OK, Create password for phpmyadmin

fix root for phpMyAdmin "sudo mysql" or "sudo mysql -uroot -p12345"
```
GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY '12345' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```

Mysql Configuration

```
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Edit 50-server.cnf

```
bind-address=0.0.0.0
max_allowed_packet=512M
max_connections=200
sudo systemctl restart mariadb.service
sudo systemctl status mariadb
```

Enable Apache Mod_Rewrite

```
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## License
DkTaP82
