---
layout: post
title: "Allow new user to login from remote in Percona server"
date: 2019-06-05T11:32:43+08:00
comments: true
external-url:
categories:
---

### Allow remote connect

```bash
vi /etc/my.cnf # bind-address = 0.0.0.0
systemctl restart mysqld
netstat -lnp | grep mysql # confirm 3306 is listened
firewall-cmd --add-service=mysql --permanent
firewall-cmd --reload
```

### Add user


```bash
mysql -u root -p # input password if required
CREATE DATABASE cybros_bi character set UTF8mb4 collate utf8mb4_bin;
CREATE USER 'cybros_bi'@'%' IDENTIFIED BY 'cybros_bi_password';
GRANT ALL ON cybros_bi.* TO 'cybros_bi'@'%';
FLUSH PRIVILEGES;
```
