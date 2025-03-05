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
echo "<h1><center>Workshop Linux Dasar></h1>" > /var/www/html/index.html
``````
* Reload configuration apache2
``````
systemctl reload apache2
``````
# Mysql Service
* Install mysql service
``````
apt install mysql -y
``````
* Start and enable mysql-server
``````
systemctl enable --now mysql-server
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
mysql> select user,host,password from mysql.user; 
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

# Instalasi Wordpress
## Pre-Install Wordpress
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
* Change owner and group
```
chown -R www-data:www-data wordpress
```
* define db,user,pass wordpress
```
cd wordpress
cp wp-config-sample.php wp-config.php
```
* Change configuration below
```
// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'rafli' );

/** Database password */
define( 'DB_PASSWORD', 'rafli123' );
```
* Configure .htaccess
```
cat << EOF >> .htaccess
> php_value upload_max_filesize = 512M
php_value post_max_size = 256M
php_value max_execution_time = 3000
php_value max_input_time = 600
php_value max_input_vars = 3000
> EOF
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
## Access Wordpress
http://ipserver
>[!NOTE]
> Intructions from trainer