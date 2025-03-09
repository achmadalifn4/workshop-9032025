# Web service
* Install apache2
``````
apt install apache2 -y
``````
* View status service
``````
systemctl status apache2
``````
* Enable and start apache2
``````
systemctl enable apache2 --now
``````
* Edit konten website with echo
``````
echo "<h1><center>Workshop Linux Dasar</h1>" > /var/www/html/index.html
``````
* Reload configuration apache2
``````
systemctl reload apache2
``````
# Mysql Service
* Install mysql service
``````
apt install mysql-server -y
``````
* Start and enable mysql-server
``````
systemctl enable --now mysql
``````
* Initial setup mysql
``````
mysql_secure_installation
``````
>[!NOTE]
> Intructions from trainer

* Connect to console mysql
``````
mysql -u root -p
``````
* Show User list in database
``````
mysql> select user,host from mysql.user; 
``````
* Creating user
``````
 mysql> create user workshop identified by 'password';
``````
* Give roles or permission to user
``````
mysql> grant all on *.* to workshop;
``````
* Flush privileges after give permission
``````
mysql> flush privileges;
``````
* Show database in host
``````
mysql> show databases; 
``````
* Create database db_word
``````
mysql> create database db_word;
``````
* Change ip database
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
# Instalasi Wordpress
* Install script install wordpress
```
wget http://lab.rafly.my.id/wordpress.sh
```
* access web browser
```
http://172.23.8.x
```
* Install Template
```
http://lab.rafly.my.id/template.zip
```



## Pre-Install Wordpress
* Install php
```
apt install php-fpm php8.1-fpm php-mysql libapache2-mod-php php8.1-xml
```
* Install unzip
```
apt install unzip
```
* Install Wordpress
```
wget https://wordpress.org/latest.zip
```
* unzip wordpress
```
unzip latest.zip
```
* Move Wordpress 
```
mv wordpress /var/www/
```
* Enter the directory
```
cd /var/www
```
* Change owner and group
```
chown -R www-data:www-data wordpress
```
* define db,user,pass wordpress
```
cd wordpress
mv wp-config-sample.php wp-config.php
```
* Change configuration below
```
// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'db_word' );

/** Database username */
define( 'DB_USER', 'workshop' );

/** Database password */
define( 'DB_PASSWORD', 'password' );
```
* Configure php.ini ini /etc/php/8.1/apache2 cli and fpm
```
cat << EOF >> php.ini
upload_max_filesize = 512M
post_max_size = 256M
max_execution_time = 3000
max_input_time = 600
max_input_vars = 3000
EOF
```
## Configure web service
* cp 000-default.conf to wordpress.conf
```
cd /etc/apache2/sites-available
cp 000-default.conf wordpress.conf
```
* Configure wordpress.conf
```
vim wordpress.conf
```
* Change configuration below
```
DocumentRoot /var/www/wordpress
```
* Activate modul rewrite
```
a2enmod rewrite
```
* Deactivate 000-default.conf
```
a2dissite 000-default.conf
```
* Create softlink
```
ln -s /etc/apache2/sites-available/wordpress.conf /etc/apache2/sites-enabled/
```
* Reload and apache2
```
systemctl reload apache2
systemctl restart apache2
```
## Access Wordpress
http://ipserver
>[!NOTE]
> Intructions from trainer