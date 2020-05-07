---
layout: post
title: "Allow new user to login from remote in Percona server"
date: 2019-06-05T11:32:43+08:00
comments: true
external-url:
categories: [MySQL]
---

### Allow remote connect

```bash
vi /etc/my.cnf # bind-address = 0.0.0.0
systemctl restart mysqld
netstat -lnp | grep mysql # confirm 3306 is listened
firewall-cmd --add-service=mysql --permanent
firewall-cmd --reload
```

### Add user with dedicated table space access


```bash
mysql -u root -p # input password if required
CREATE DATABASE cybros_bi character set UTF8mb4 collate utf8mb4_bin;
CREATE USER 'cybros_bi'@'%' IDENTIFIED BY 'cybros_bi_password';
GRANT ALL ON cybros_bi.* TO 'cybros_bi'@'%';
FLUSH PRIVILEGES;
```

## Add new user but only read only table access.

```bash
mysql -u root -p # input password if required
CREATE USER 'cad_reader'@'%' IDENTIFIED BY 'cad_reader_password';
GRANT SELECT ON cybros_prod.cad_operations TO 'cad_reader'@'%';
GRANT SELECT ON cybros_prod.cad_sessions TO 'cad_reader'@'%';
GRANT SELECT ON cybros_prod.users TO 'cad_reader'@'%';
GRANT SELECT ON cybros_prod.department_users TO 'cad_reader'@'%';
GRANT SELECT ON cybros_prod.departments TO 'cad_reader'@'%';
FLUSH PRIVILEGES;
```

## Show user access

```bash
show grants for 'sync_user_read_only'@'%';
```
