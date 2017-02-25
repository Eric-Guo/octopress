---
layout: post
title: "Deploy ruby-china as a new forum software to Aliyun"
date: 2016-09-04T13:08:45+08:00
comments: true
external-url: 
categories: [Rails, Ruby]
---

This wiki based on the Ubuntu 14.04.5 LTS Server.

# Prerequisites

```bash
apt-get update && apt-get upgrade
apt-get dist-upgrade
apt-get install software-properties-common htop curl git
```

## [memcached](https://launchpad.net/ubuntu/+source/memcached)

```bash
apt-get install memcached
```

## [Redis](https://launchpad.net/~chris-lea/+archive/ubuntu/redis-server)

```bash
add-apt-repository ppa:chris-lea/redis-server
apt-get update && apt-get install redis-server
```

## [PostgreSQL](https://www.postgresql.org/download/linux/ubuntu/)

```bash
echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
apt-get update && apt-get install postgresql
apt-get install libpq-dev
```

## [imagemagick](https://launchpad.net/ubuntu/+source/imagemagick)

```bash
apt-get install imagemagick
```

## [node.js](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)

```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
apt-get install -y nodejs build-essential
```

## [Oracle Java](https://launchpad.net/~webupd8team/+archive/ubuntu/java)

```bash
add-apt-repository ppa:webupd8team/java
apt-get update
apt-get install oracle-java8-installer
```

## [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-repositories.html#_apt)

```bash
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | apt-key add -
echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
apt-get update && apt-get install elasticsearch
update-rc.d elasticsearch defaults
service elasticsearch start
```

## [RVM](https://rvm.io)

```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
```

## Ruby 2.4.0

```bash
rvm requirements
echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db # if need mirror.
rvm install 2.4.0 --disable-binary
rvm use 2.4.0 --default
echo "gem: --no-document" >> /etc/gemrc
echo "gem: --no-document" >> ~/.gemrc
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/ # if need mirror
gem install bundler
```

## [Nginx](https://launchpad.net/~nginx/+archive/ubuntu/stable)

```bash
add-apt-repository ppa:nginx/stable
apt-get update && apt-get install nginx
```

# Create deploy user

```bash
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'deploy user' \
  --group \
  --disabled-password \
  --home /data/www deploy
mkdir /data/www/.ssh
cp ~/.ssh/authorized_keys /data/www/.ssh/
chown deploy:deploy -R .ssh/
```

# Prepare setting files while doing cap deploy

```bash
cap production deploy
cp config/secrets.yml.default  config/secrets.yml
cp config/database.yml.default config/database.yml
cp config/config.yml.default   config/config.yml
cp config/redis.yml.default    config/redis.yml
cp config/puma.example.rb      config/puma-web.rb
cp config/elasticsearch.yml.default config/elasticsearch.yml 
```

# Change as necessory

```bash
vi /data/www/ruby-china/shared/config/database.yml
vi /data/www/ruby-china/shared/config/config.yml
vi /data/www/ruby-china/shared/config/secrets.yml
```

notice SECRET_KEY_BASE can get from `rails secret`

# Create DB

```bash
sudo su - postgres
psql
CREATE ROLE deploy;
ALTER ROLE deploy LOGIN;
CREATE DATABASE ftghub_prod WITH ENCODING='UTF8' OWNER='deploy';
vi /etc/postgresql/9.6/main/pg_hba.conf
```

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local  	ftghub_prod    	deploy 					                peer
```

```bash
service postgresql restart
```

# Deploy

```
cap production deploy
```

# Mapping nginx to puma

```nginx
upstream ruby_china_backend {
    # This is the socket we configured in unicorn.rb
    server unix:///data/www/ruby-china/shared/tmp/sockets/puma.sock fail_timeout=0;
    keepalive 3;    
}
server {
    listen 80;
    server_name ftghub.com;

    root /www/ruby-china/current/public;
    gzip_static on; #to serve pre-gzipped version

    if (-f $document_root/system/maintenance.html) {
      rewrite ^(.*)$ /system/maintenance.html break;
    }

    location ~ (/assets|/system|/avatar.png|/favicon.ico|/*.txt) {
      access_log        off;
      expires           14d;
      gzip_static on;
      add_header  Cache-Control public;
    }

    location / {
      if ($host != 'ftghub.com') {
        rewrite ^/(.*)$ http://ftghub/$1 permanent;
      }
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
      proxy_pass         http://ruby_china_backend;
      gzip on;
    }

    error_page 500 502 503 504 /500.html;
        location = /500.html {
        root /www/ruby-china/current/public;
    }
}
```

# Running DB seed

```bash
bundle exec rake db:seed RAILS_ENV=production
```

# Reindex ElasticSearch

Must ensure those model at least having one record each.

```bash
rake environment elasticsearch:import:model CLASS=Page FORCE=y
rake environment elasticsearch:import:model CLASS=Topic FORCE=y
rake environment elasticsearch:import:model CLASS=User FORCE=y
```
