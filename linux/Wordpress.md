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
echo "<h1><center>Belajar Web Service LSA IDN</center></h1>" > /var/www/html/index.html
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
