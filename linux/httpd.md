![image](https://github.com/user-attachments/assets/5cc85905-40e9-4a43-b12d-85310f989fb5)![image](https://github.com/user-attachments/assets/083176b3-368f-4a22-857c-d4138f345d81)# Instalasi
Jalankan perintah dibawah untuk instalasi web server httpd.
```bash
[root@instance ~]# dnf update
[root@instance ~]# dnf install httpd -y
[root@instance ~]# systemctl enable --now httpd
```
Izinkan incoming port http
```bash
[root@instance ~]# firewall-cmd --permanent --add-service=http
[root@instance ~]# firewall-cmd --reload
```
Uji coba di client akses melalui browser, akses http://35.202.243.147.
![img](https://i.imgur.com/R9l9Vfe.png)
# Instalasi PHP
```bash
[root@instance ~]# dnf install php php-{fpm,mysqlnd} -y
[root@instance ~]# systemctl enable --now php-fpm
```
Buat script phpinfo.
```bash
[root@instance ~]# cat << EOF > /var/www/html/phpinfo.php
<?php
phpinfo();
?>
EOF
```
Uji coba di client akses melalui browser, akses http://35.202.243.147/phpinfo.php.
![img](https://i.imgur.com/GZntz45.png)
# Userdir
Aktifkan module userdir.
```bash
[root@instance ~]# semanage boolean -m --on httpd_enable_homedirs
```
Edit file `/etc/httpd/conf.d/userdir.conf`.
```bash
[root@instance ~]# nano /etc/httpd/conf.d/userdir.conf
    #UserDir disabled
    UserDir public_html
```
Setelah itu tambahkan user baru.
```bash
[root@instance ~]# adduser asrul
[root@instance ~]# su - asrul
[asrul@instance ~]# chmod 711 $HOME
[asrul@instance ~]# mkdir public_html
[asrul@instance ~]# chmod 755 public_html
[asrul@instance ~]# cat << EOF > public_html/index.html
<h1>Welcome to $(whoami)'s site.</h1>
EOF
```
```bash
[root@instance ~]# adduser rafli
[root@instance ~]# su - rafli
[rafli@instance ~]# chmod 711 $HOME
[rafli@instance ~]# mkdir public_html
[rafli@instance ~]# chmod 755 public_html
[rafli@instance ~]# cat << EOF > public_html/index.html
<h1>Welcome to $(whoami)'s site.</h1>
EOF
```
Uji coba di client akses melalui browser, akses http://35.202.243.147/~asrul.
![img](https://i.imgur.com/QNkRR9P.png)
Uji coba di client akses melalui browser, akses http://35.202.243.147/~rafli.
![img](https://i.imgur.com/7Qao0CS.png)
# Basic Authentication
Buat file .htaccess pada masing masing user.
```bash
[asrul@instance ~]# cat << EOF > public_html/.htaccess
AuthType Basic
AuthName "Basic Authentication"
AuthUserFile /home/asrul/public_html/.htpasswd
Require valid-user
EOF
[asrul@instance ~]# htpasswd -c public_html/.htpasswd asrul
New password: 
Re-type new password: 
Adding password for user asrul
```
```bash
[rafli@instance ~]# cat << EOF > public_html/.htaccess
AuthType Basic
AuthName "Basic Authentication"
AuthUserFile /home/rafli/public_html/.htpasswd
Require valid-user
EOF
[asrul@instance ~]# htpasswd -c public_html/.htpasswd rafli
New password: 
Re-type new password: 
Adding password for user rafli
```
Uji coba di client akses melalui browser menggunakan user dan password yang benar, akses http://35.202.243.147/~asrul.
![img](https://i.imgur.com/kGZR849.png)
![img](https://i.imgur.com/QNkRR9P.png)
# VirtualHost
Buat konfigurasi virtualhost di `/etc/httpd/conf.d/asrul.net.conf`.
```bash
[root@instance ~]# cat << EOF > /etc/httpd/conf.d/asrul.net.conf
<VirtualHost *:80>
    ServerAdmin webmaster@asrul.net
    DocumentRoot "/home/asrul/public_html"
    ServerName asrul.net
    ErrorLog "/var/log/httpd/asrul.net-error_log"
    CustomLog "/var/log/httpd/asrul.net-access_log" common
</VirtualHost>
EOF
```
Restart service httpd.
```
[root@instance ~]# systemctl restart httpd
```
Uji coba akses melalui browser http://asrul.net.
![img](https://i.imgur.com/q7vNr1B.png)
# CA
Buat file `/etc/ssl/config.txt`.
```bash
[root@instance ssh]# cd /etc/ssl
[root@instance ssl]# nano config.txt
................................................................
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = asrul.net
DNS.2 = *.asrul.net
................................................................
```
Buat ca key.
```bash
[root@instance ssl]# openssl genrsa -des3 -out ca.key 2048
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
```
Buat ca certificate.
```bash
[root@instance ssl]# openssl req -x509 -new -nodes -key ca.key -sha256 -days 365 -out ca.pem
Enter pass phrase for ca.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:ID
State or Province Name (full name) []:JAKARTA
Locality Name (eg, city) [Default City]:WEST JAKARTA
Organization Name (eg, company) [Default Company Ltd]:IDN
Organizational Unit Name (eg, section) []:SYSADMIN
Common Name (eg, your name or your server's hostname) []:CA IDN
Email Address []:admin@example.com
```
Buat server key.
```bash
[root@instance ssl]# openssl genrsa -out domain.key 2048
```
Buat server certificate sign request.
```bash
[root@instance ssl]# openssl req -new -key domain.key -out domain.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:ID
State or Province Name (full name) []:JAKARTA                     
Locality Name (eg, city) [Default City]:WEST JAKARTA
Organization Name (eg, company) [Default Company Ltd]:IDN
Organizational Unit Name (eg, section) []:SYSADMIN
Common Name (eg, your name or your server's hostname) []:asrul.net
Email Address []:admin@asrul.net

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```
Sign csr untuk generate server certificate.
```bash
[root@instance ssl]# openssl x509 -req -in domain.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out domain.crt -days 365 -sha256 -extfile config.txt
Certificate request self-signature ok
subject=C=ID, ST=JAKARTA, L=WEST JAKARTA, O=IDN, OU=SYSADMIN, CN=asrul.net, emailAddress=admin@asrul.net
Enter pass phrase for ca.key:
```
File domain.key dan domain.crt sudah di generate.
```bash
[root@instance ssl]# ls domain.{key,crt}
domain.crt  domain.key
```
# HTTPS
Install module ssl untuk httpd.
```bash
[root@instance ssl]# dnf install mod_ssl -y
```
Izinkan incoming port https
```bash
[root@instance ~]# firewall-cmd --permanent --add-service=https
[root@instance ~]# firewall-cmd --reload
```
Buat konfigurasi virtualhost untuk https di `/etc/httpd/conf.d/ssl-asrul.net.conf`.
```bash
[root@instance ~]# cat << EOF > /etc/httpd/conf.d/ssl-asrul.net.conf
<VirtualHost *:443>
    ServerAdmin webmaster@asrul.net
    DocumentRoot "/home/asrul/public_html"
    ServerName asrul.net
    SSLEngine on
    SSLCertificateFile /etc/ssl/domain.crt
    SSLCertificateKeyFile /etc/ssl/domain.key
    ErrorLog "/var/log/httpd/ssl-asrul.net-error_log"
    CustomLog "/var/log/httpd/ssl-asrul.net-access_log" common
</VirtualHost>
EOF
```
Restart service httpd.
```
[root@instance ~]# systemctl restart httpd
```
Uji coba akses melalui browser https://asrul.net.
![image](https://github.com/user-attachments/assets/af9590ea-e2bf-491c-a834-7a3ed8d09871)
Tapi masih terdapat warning dikarenakan ca masih belum terinstall di komputer client. Transfer file ca.pem ke komputer client dan import di browser.
![image](https://github.com/user-attachments/assets/d7ab4374-eb34-41a8-b4ef-a1ee4365c719)
