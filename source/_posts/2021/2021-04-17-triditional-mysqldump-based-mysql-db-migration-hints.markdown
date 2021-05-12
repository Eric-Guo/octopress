---
layout: post
title: "Triditional mysqldump based MySQL DB migration hints"
date: 2021-04-17T11:11:32+08:00
comments: true
external-url:
categories: [MySQL]
---

Dump based on the SQL not fast, but many system doesn't need either, and the simplicity many times shining instead of faster.

## Dump the DB out

```bash
mysqldump -u root -p --all-databases | gzip > thape_bidb_all.sql.gz # input password
```

It's the slowest part, so maybe `Ctrl+Z` and `bg` and `disown -h %1` and having a good sleep.

## Transfer DB

```bash
scp thape_bidb_all.sql.gz target_server:.
```

## Import DB


```
unzip thape_bidb_all.sql.gz
mysql -u root -p
source thape_bidb_all.sql
\q
```

## Make analyze and optimize

```bash
mysqlcheck -u root -p --auto-repair --optimize --all-databases
```

[Oritinal source](https://stackoverflow.com/questions/5474662/mysql-optimize-all-tables)
