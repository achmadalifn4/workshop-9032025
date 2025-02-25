# Instalasi
```bash
[root@instance ~]# dnf install openssh-server
[root@instance ~]# systemctl enable --now sshd
[root@instance ~]# systemctl status sshd
```
Izinkan incoming port ssh
```bash
[root@instance ~]# firewall-cmd --permanent --add-service=ssh
[root@instance ~]# firewall-cmd --reload
```
Uji coba di client
```bash
[asrul@client ~]# ssh root@35.202.243.147
```
# SFTP
Uji coba di client
```bash
[asrul@client ~]# sftp root@35.202.243.147
```
Disable sftp
```bash
[root@instance ~]# nano /etc/ssh/sshd_config
................................................................
# Cari line dibawah ini
Subsystem      sftp    /usr/libexec/openssh/sftp-server
# Replace dengan line ini
Subsystem       sftp    /bin/false
................................................................
```
Restart service sshd
```bash
[root@instance ~]# systemctl restart sshd
[root@instance ~]# systemctl status sshd
```
Uji coba di client
```bash
[asrul@client ~]# sftp root@35.202.243.147
```
# Security Hardening
```bash
Port 2025
AllowUsers siswa1 siswa2
AllowGroups pelajar
DenyUsers guru1 guru2
DenyGroups pengajar
```
Restart service sshd
```bash
[root@instance ~]# systemctl restart sshd
[root@instance ~]# systemctl status sshd
```
