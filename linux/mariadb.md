# Instalasi
```bash
[root@instance ~]# dnf install mariadb-server -y
[root@instance ~]# systemctl enable --now mariadb
```
# Initial Setup
```
[root@instance ~]# mariadb-secure-installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
# Basic Command
## Login ke MariaDB
```
mariadb -u username -p
```

## Membuat Database
```bash
MariaDB [(none)]> CREATE DATABASE db_wordpress;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> CREATE DATABASE db_test;
Query OK, 1 row affected (0.001 sec)
```

## Menampilkan List Database
```bash
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db_test            |
| db_wordpress       |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
5 rows in set (0.001 sec)
```

## Memilih Database
```bash
MariaDB [(none)]> USE db_test
Database changed
MariaDB [db_test]> 
```

## Membuat Tabel
```bash
MariaDB [db_test]> CREATE TABLE list_users (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> username VARCHAR(255),
    -> uid VARCHAR(20),
    -> role VARCHAR(100)
    -> );
Query OK, 0 rows affected (0.005 sec)
```

## Menampilkan List Tabel
```bash
MariaDB [db_test]> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| list_users        |
+-------------------+
1 row in set (0.001 sec)
```

## Memasukkan Data
```bash
MariaDB [db_test]> INSERT INTO list_users (username, uid, role) VALUES
    -> ('Parto', '00001', 'Admin'),
    -> ('Tukiyem', '00002', 'Regular'),
    -> ('Dono', '00003', 'Admin'),
    -> ('Dina', '00004', 'Regular');
Query OK, 4 rows affected (0.004 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

## Mengambil Data
```bash
MariaDB [db_test]> SELECT * FROM list_users;
+----+----------+-------+---------+
| id | username | uid   | role    |
+----+----------+-------+---------+
|  1 | Parto    | 00001 | Admin   |
|  2 | Tukiyem  | 00002 | Regular |
|  3 | Dono     | 00003 | Admin   |
|  4 | Dina     | 00004 | Regular |
+----+----------+-------+---------+
4 rows in set (0.001 sec)

MariaDB [db_test]> SELECT * FROM list_users WHERE role = 'Admin';
+----+----------+-------+-------+
| id | username | uid   | role  |
+----+----------+-------+-------+
|  1 | Parto    | 00001 | Admin |
|  3 | Dono     | 00003 | Admin |
+----+----------+-------+-------+
2 rows in set (0.001 sec)
```

## Mengupdate Data
```bash
MariaDB [db_test]> UPDATE list_users SET role = 'Admin' WHERE uid = '00004';
Query OK, 1 row affected (0.002 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [db_test]> select * from list_users;
+----+----------+-------+---------+
| id | username | uid   | role    |
+----+----------+-------+---------+
|  1 | Parto    | 00001 | Admin   |
|  2 | Tukiyem  | 00002 | Regular |
|  3 | Dono     | 00003 | Admin   |
|  4 | Dina     | 00004 | Admin   |
+----+----------+-------+---------+
4 rows in set (0.001 sec)
```

## Menghapus Data
```bash
MariaDB [db_test]> DELETE FROM list_users WHERE username = 'Dina';
Query OK, 1 row affected (0.002 sec)

MariaDB [db_test]> select * from list_users;
+----+----------+-------+---------+
| id | username | uid   | role    |
+----+----------+-------+---------+
|  1 | Parto    | 00001 | Admin   |
|  2 | Tukiyem  | 00002 | Regular |
|  3 | Dono     | 00003 | Admin   |
+----+----------+-------+---------+
3 rows in set (0.001 sec)
```

## Menghapus Tabel
```bash
MariaDB [db_test]> DROP TABLE list_users;
Query OK, 0 rows affected (0.004 sec)

MariaDB [db_test]> SHOW TABLES;
Empty set (0.001 sec)
```

## Menghapus Database
```bash
MariaDB [db_test]> DROP DATABASE db_test;
Query OK, 0 rows affected (0.003 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| db_wordpress       |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)
```

## Membuat User Baru
```bash
MariaDB [(none)]> CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'resupw';
Query OK, 0 rows affected (0.003 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON db_wordpress.* TO 'wpuser'@'localhost';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> SELECT User, Host FROM mysql.user;
+-------------+-----------+
| User        | Host      |
+-------------+-----------+
| mariadb.sys | localhost |
| mysql       | localhost |
| root        | localhost |
| wpuser      | localhost |
+-------------+-----------+
4 rows in set (0.002 sec)
```

## Menghapus User
```bash
MariaDB [(none)]> DROP USER 'wpuser'@'localhost';
```

# Backup & Restore (ISSUE on Physical)
## Logical Backup & Restore
```
# Backup
[root@instance ~]# mariadb-dump db_wordpress > db_wordpress.sql

# Restore
[root@instance ~]# mariadb-dump db_wordpress < db_wordpress.sql
```

## Physical Backup & Restore
```
# Backup
[root@instance ~]# mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=root --password=asrul

# Restore
[root@instance ~]# mariabackup --prepare \
   --target-dir=/var/mariadb/backup/
[root@instance ~]# mariabackup --copy-back \
   --target-dir=/var/mariadb/backup
```
