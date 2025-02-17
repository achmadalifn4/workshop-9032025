# Linux

# Subscription-Manager

## Register 

untuk bisa menggunakan server redhat, system harus terhubung dengan subscription rhel, kita bisa menggunakan daftar subscription secara free dengan join redhat developer. untuk mendaftarnya bisa masuk pada link [Redhat Developer](https://developers.redhat.com/) 
<p align="center">
  <img src="/images/image6.png" />
</p>
isi form register untuk join redhat developer, jika sudah bisa cek di verifikasi email. dan setelah itu login di
<p align="center">
  <img src="/images/image5.png" />
</p>
setelah login anda harus mengisi form lagi untuk melengkapi pembuatan RedHat Account
<p align="center">
  <img src="/images/image8.png" />
</p>
untuk tampilan login seperti dibawah ini
<p align="center">
  <img src="/images/image9.png" />
</p>
# Basic Commands
## Basic Operation Linux
displays the user used
```
whoami
```
displays the currently logged in user
```
who
```
displays the server hostname
```
hostnamectl
```
displays date and time
```
timedatectl
```

## Assesmen Server
displays the operating system
```
cat /etc/os-release 
```
displays the operating system
```
uname
```
displays the server processor
```
lscpu
```
display memory
```
free -h
```
display storage
```
lsblk / df -h
```

# File and Directory Management


## Directory Management
create dirctory
```
mkdir idn
```
enter and exit the directory
```
cd idn
```
displays the contents of the directory
```
ls
```
displays which directory we are in
```
pwd
```
delete directory
```
rmdir idn
```

## File management
create file without content
```
touch idn-bootcamp
```
create file with content
```
echo "Hello World" >> idn_bootcamp
```
displays the contents of the file
```
cat idn_bootcamp
```
displays the first 10 lines of a file
```
head /var/log/syslog
```
displays the last 10 lines of a file
```
tail /var/log/syslog
```
delete file
```
rm idn-bootcamp
```
copy file and dirctory
```
cp idn_bootcamp ~/sysadmin
```
move or rename file and directory
```
mv idn_bootcamp nama_peserta
```

# User and Group Management

## User Management
add new user
```
useradd nama_peserta
```
displays a list of users
```
cat /etc/passwd
```
change user password
```
passwd nama_user
```
modify user
```
usermod -u 10000 rudi
usermod -aG nama_group nama_user
```

## Group Management
create new group
```
groupadd idn
```
displays a list of group
```
cat /etc/group
```
modify group
```
groupmod -n old_name new_name
```
delete group
```
groupdel group_name
```
# File Permsision and Ownership
modify file permssion
```
chmod 640 file_name
```
change ownership
```
chown user_name file_name
```
change group file
```
chgrp group_name file_name
```

# Basic Settings
set hostname
```
hostnamectl set-hostname <hostname>
```
set timezone
```
timedatectl set-timezone Asia/Jakarta
```
network settings
```
nmtui
```
untuk mengeditnya pilih opsi 'Edit a conection'
<p align="center">
  <img src="/images/image.png" />
</p>
pilih pada network yang ingin diedit
<p align="center">
  <img src="/images/image1.png" />
</p>
pastikan bahwa ip yang digunakan 'available' dan jika sudah bisa klik ok
<p align="center">
  <img src="/images/image2.png" />
</p>
jika sudah bisa klik tombol 'Esc' pada pojok kiri keyboard atau klik back

dan untuk mengaktifkan konfigurasinya pilih opsi 'Activate a connection'. dan klik 'deactivate dan activate' untuk merestart konfigurasi networknya
<p align="center">
  <img src="/images/image3.png" />
</p>

```bash
[root@rhel9 ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens34: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:bd:b3:fa brd ff:ff:ff:ff:ff:ff
    altname enp2s2
    inet 172.23.15.1/20 brd 172.23.15.255 scope global noprefixroute ens34
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:febd:b3fa/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
# Remote access
install ssh
```
dnf install openssh-server
```
enable and start ssh
```
systemctl enable --now sshd.service
```

# Firewalld
check firewall
```
systemctl status firewalld
```
enable firewalld
```
systemctl enable firewalld
```
allow port 80
```
firewall-cmd --permanent --zone=public --add-port=80/tcp
```
allow port 443
```
firewall-cmd --permanent --zone=public --add-port=443/tcp
```
reload firewalld
```
firewall-cmd --reload
```
check firewalld
```
firewall-cmd --list-all
```

# cronjob
create job
```
crontab -u user_name -e

* * * * echo "Hello World" > /root/master/file-backup.txt
```
list user's crontab
```
crontab -u user_name -l
```

#Selinux

untuk melihat type selinux yang sedang digunakan
```
getenforce
```

untuk mengganti type selinux bisa edit dibawah ini pada bagian "SELINUX=".
```
vim /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
# See also:
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/using_selinux/changing-selinux-states-and-modes_using-selinux#changing-selinux-modes-at-boot-time_changing-selinux-states-and-modes
#
# NOTE: Up to RHEL 8 release included, SELINUX=disabled would also
# fully disable SELinux during boot. If you need a system with SELinux
# fully disabled instead of SELinux running with no policy loaded, you
# need to pass selinux=0 to the kernel command line. You can use grubby
# to persistently set the bootloader to boot with selinux=0:
#
#    grubby --update-kernel ALL --args selinux=0
#
# To revert back to SELinux enabled:
#
#    grubby --update-kernel ALL --remove-args selinux
#
SELINUX=enforcing
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
selain edit dalam file, bisa juga menggunakan 
```
setenforce 0 > untuk Permissive
setenforce 1 > untuk Enforcing
```