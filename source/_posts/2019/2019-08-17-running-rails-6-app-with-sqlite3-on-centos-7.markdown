---
layout: post
title: "Running Rails 6 app with sqlite3 on CentOS 7"
date: 2019-08-17T08:57:56+08:00
comments: true
external-url:
categories: [Rails, SQLite]
---

Running CentOS 7 as operation system for Rails is very stable, but such stability comes with stagnation, for example, the sqlite3 version is 3.7 instead of 3.8, so cause Rails 6 [refused to run](https://github.com/rails/rails/blob/v6.0.0/activerecord/lib/active_record/connection_adapters/sqlite3_adapter.rb#L334) in CentOS 7 withouth install a third party sqlite3 version.

So a quickly fix is install [atomic sqlite](https://centos.pkgs.org/7/atomic-x86_64/atomic-sqlite-devel-3.8.5-6803.el7.art.x86_64.rpm.html) and setting the correct build options.

```bash
$HOME/.rbenv/bin/rbenv exec bundle config build.sqlite3 "--with-sqlite3-include=/opt/atomic/atomic-sqlite/root/usr/include --with-sqlite3-lib=/opt/atomic/atomic-sqlite/root/usr/lib64 --with-sqlite3-dir=/opt/atomic/atomic-sqlite/root/usr"
```

Maybe also need to do a patch but any way, I successfully [upgrade to Rails 6.](https://github.com/thape-cn/website/commit/8bcf002ed26f75dae18d46f10c93ea080fac3e40)
