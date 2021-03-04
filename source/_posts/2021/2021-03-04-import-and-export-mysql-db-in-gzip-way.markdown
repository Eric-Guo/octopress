---
layout: post
title: "Import and export mysql DB in gzip way"
date: 2021-03-04T14:46:39+08:00
comments: true
external-url: 
categories: [MySQL]
---

# Export MySQL in gzip format

```
mysqldump -u <YOUR USERNAME> -p<PASSWORD> <YOUR DBNAME> | gzip > mysql_cybros_db_`date +%Y%m%d%H%M`.sql.gz

```
# Import directly in gzip format

```bash
mysql -u root
DROP DATABASE thape_cybros_dev;
CREATE DATABASE thape_cybros_dev character set UTF8mb4 collate utf8mb4_bin;
\q
# gunzip < mysql_cybros_db.sql.gz | mysql -u root -pPASSWORD thape_cybros_dev
gunzip < mysql_cybros_db.sql.gz | mysql -u root thape_cybros_dev
```
