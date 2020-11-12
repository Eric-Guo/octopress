---
layout: post
title: "Install PostgreSQL 13 and correct ruby pg gem support"
date: 2020-11-12T08:35:37+08:00
comments: true
external-url: 
categories: [Rails, Ruby]
---

## Install postgresql13

Installation is similar to [postgresql12](/2020/05/16/fix-yum-update-postgresql12-to-v12-dot-3-require-llvm-toolset-7-clang-equals-4-dot-0-1-dependency-problem/).

```bash
# postgresql v12.3+ require LLVM-toolset-7-clang
sudo yum install centos-release-scl-rh
sudo yum install llvm-toolset-7-clang
# following https://www.postgresql.org/download/linux/redhat/
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum install -y postgresql13-server
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
sudo systemctl enable postgresql-13
sudo systemctl start postgresql-13
# Checking DB status
sudo systemctl status postgresql-13.service
```

## Install pg gem correctly

```bash
bundle config build.pg --with-pg-config=/usr/pgsql-13/bin/pg_config
bundle install
```
