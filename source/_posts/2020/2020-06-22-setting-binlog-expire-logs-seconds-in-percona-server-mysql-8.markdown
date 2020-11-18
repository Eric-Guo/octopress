---
layout: post
title: "Setting binlog_expire_logs_seconds in Percona-Server MySQL 8"
date: 2020-06-22T10:12:44+08:00
comments: true
external-url:
categories: [MySQL]
---

`vi /etc/my.conf`

```text my.conf
binlog_expire_logs_seconds=1296000
```

systemctl stop mysqld
systemctl start mysqld

```sql
select now();
show global variables like 'binlog_expire_logs_seconds';
```
