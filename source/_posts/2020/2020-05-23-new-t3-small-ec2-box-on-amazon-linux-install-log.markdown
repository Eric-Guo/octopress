---
layout: post
title: "New T3 micro EC2 box on Amazon Linux install log"
date: 2020-05-23T10:17:14+08:00
comments: true
external-url:
categories:
---

Time is pass so faster, another 3 years passed after [my previous T2 micro installed](/2017/05/29/new-ec2-box-on-amazon-linux-install-log/).

Now have to install another new box with price $134 in Tokyo AWS.

## Install postgresql 11

```bash
yum update
amazon-linux-extras install postgresql11 epel
yum install -y postgresql-server postgresql-devel
/usr/bin/postgresql-setup --initdb
systemctl enable postgresql
systemctl start postgresql
sudo -u postgres -i psql -c 'SELECT version();'
```

## Install nginx

```bash
amazon-linux-extras install nginx1
yum install nginx
systemctl enable nginx
systemctl start nginx
```

## Install node.js 14 & yarn

```bash
curl --silent --location https://rpm.nodesource.com/setup_14.x | bash -
yum install -y gcc-c++ make
yum install -y nodejs
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

## Install dependencies required by rbenv and ruby 2.7.1

```bash
yum install openssl-devel readline-devel zlib-devel gdbm-devel
```
