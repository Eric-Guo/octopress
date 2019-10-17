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
