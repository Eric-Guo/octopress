---
layout: post
title: "Upgrade brew installed mysql 8.0 inplace"
date: 2019-01-24T11:57:33+08:00
comments: true
external-url:
categories:
---

Largely based on [this blog](https://mysqlserverteam.com/inplace-upgrade-from-mysql-5-7-to-mysql-8-0/)

Only one line required

```bash
mysql_upgrade --user=root
```

The sad news are [sequelpro not support mysql 8.0](https://github.com/sequelpro/sequelpro/issues/2699), so may have to using [TablePlus](https://github.com/TablePlus/TablePlus) instead.
