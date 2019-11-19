---
layout: post
title: "Ubuntu 18.04 ruby on rails installation log"
date: 2019-10-17T19:50:54+08:00
comments: true
external-url:
categories:
---

## Enable the Firewall

```bash
ufw app list
ufw allow ssh
ufw enable
ufw status
```

## Install node.js

```bash
apt install nodejs
apt install npm
curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
bash nodesource_setup.sh
apt-get install -y nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
apt update
apt install yarn
```

## Install rbenv

```bash
apt update
apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev
apt install git
logout # as ubuntu
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
type rbenv
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
rbenv install -l
```

## Install ruby 2.6.5

```bash
rbenv install 2.6.5
rbenv global 2.6.5
echo "gem: --no-document" > ~/.gemrc
gem install bundler
gem install rails
rbenv rehash
```

## Install postgresql

```bash
apt-get install postgresql
apt-get install postgresql-server-dev-all
sudo su - postgres
createuser ubuntu --pwprompt
psql
ALTER ROLE ubuntu LOGIN;
CREATE DATABASE cam_price_prod WITH ENCODING='UTF8' OWNER=ubuntu;
```

## Install nginx

```bash
apt-get install nginx
```

## Generate ssh key

```bash
ssh-keygen
sudo mkdir /var/www
cd /var/www
sudo mkdir cam_price
sudo chown ubuntu:ubuntu cam_price/
```


## Install HTTPS

Largely following [certbot guide](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

```
apt-get update
apt-get install software-properties-common
add-apt-repository universe
add-apt-repository ppa:certbot/certbot
apt-get update
apt-get install certbot python-certbot-nginx
```
