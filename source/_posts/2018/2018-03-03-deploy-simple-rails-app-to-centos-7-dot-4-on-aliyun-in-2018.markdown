---
layout: post
title: "Deploy simple rails app to CentOS 7.4 on Aliyun in 2018"
date: 2018-03-03T20:22:39+08:00
comments: true
external-url: 
categories: 
---

It's 2018, docker quite mature, but since we can buy a server less than 600 RMB in Aliyun including 40Gb storage, 1MB network, 1 core CPU and 2G memory, so I still want to install it in the triditional way.

# Install software in root account

## Update system

```bash
yum update
yum install htop
yum install git
reboot
```

## Install RVM

### Resolve can not import from hkp://keys.gnupg.net

In a server which can running below cmd:

```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
gpg --export --armor D39DC0E3
gpg --export --armor 39499BDB
```

In Aliyun server:

```bash
gpg --import -
```

and copy and paste the can run server public key content and press `Ctrl+D`

### Install RVM

```
\curl -sSL https://get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
logout
```

### Update RVM to master

```bash
rvm get master
rvm list known # should see ruby 2.5
```

## Install Ruby 2.5

```bash
rvm install ruby-2.5
echo "gem: --no-document" >> /etc/gemrc
echo "gem: --no-document" >> ~/.gemrc
```

## Install node.js

```bash
yum install nodejs
node --version # v6.12.3
```

## Install yarn

```bash
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

## Install nginx

```bash
yum install nginx
```

# Normal user

## Create new user - harman

```
adduser --home-dir /data/www/harman harman -g rvm
```

## Copy authorized_keys

```
mkdir /data/www/harman/.ssh
sudo cp ~/.ssh/authorized_keys /data/www/harman/.ssh/
sudo chown harman:rvm /data/www/harman/.ssh/
```

# Do capistrano deploy

# Change nginx

Modify nginx

Add below to http section and comment out default server section.

```nginx
http {
    log_format timed_combined '$remote_addr - $remote_user [$time_local] '
                          '"$request" $status $body_bytes_sent '
                          '"$http_referer" "$http_user_agent" '
                          '$request_time $upstream_response_time $pipe';

    access_log  /var/log/nginx/access.log timed_combined;
}
```

Create `/etc/nginx/conf.d/harman.conf`

```nginx
upstream harman_web {
  server unix:/data/www/harman/shared/tmp/unicorn.socket fail_timeout=0;
  keepalive 3;
}

server {
  listen 80 default_server;
  # server_name harman.bayekeji.com;

  location /nginx_status {
    allow 127.0.0.1;
    deny all;
    stub_status on;
  }

  root /data/www/harman/current/public;

  access_log /data/www/harman/shared/log/harman-access.log timed_combined buffer=1k;
  error_log /data/www/harman/shared/log/harman-error.log;

  if (-f $document_root/system/maintenance.html) {
    rewrite ^(.*)$ /system/maintenance.html break;
  }

  location ~ (/assets|/uploads|/system|/favicon.ico|/*.txt) {
    access_log        off;
    expires           14d;
    gzip_static on;
    add_header  Cache-Control public;
  }

  location / {
    proxy_redirect     off;
    proxy_set_header   Host $host;
    proxy_set_header   X-Forwarded-Host $host;
    proxy_set_header   X-Forwarded-Server $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_buffering    on;
    proxy_http_version 1.1;
    proxy_set_header   Upgrade $http_upgrade;
    proxy_set_header   Connection "Upgrade";
    proxy_set_header   X-Forwarded-Proto https;
    proxy_pass         http://harman_web;
    gzip on;
  }
}

```
