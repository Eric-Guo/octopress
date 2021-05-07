---
layout: post
title: "Deploy new rails 6 application in Ubuntu 20.04"
date: 2021-04-29T13:31:39+08:00
comments: true
external-url: 
categories: [Rails Ubuntu]
---

# Create new user

```bash
adduser cybros_vendor
sudo su - cybros_vendor
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys # and paste your public key
chmod 600 .ssh/authorized_keys
```
# Install nginx

```bash
sudo apt-get install nginx
```

# Install node.js 15 and yarn 1.x

Using [nodesource distribution](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)

```bash
curl -fsSL https://deb.nodesource.com/setup_15.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install gcc g++ make
## To install the Yarn package manager
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

# Install rbenv and Ruby 3.0.1

```bash
sudo apt install rbenv
sudo su - cybros_vendor
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
rbenv install 3.0.1
rbenv global 3.0.1
echo "gem: --no-document" > ~/.gemrc
eval "$(rbenv init -)" >> ~/.bash_profile # or past the `rbenv init -`
rbenv shell 3.0.1
```

# Install MySQL Client

```bash
sudo apt-get install gnupg2
wget https://repo.percona.com/apt/percona-release_latest.$(lsb_release -sc)_all.deb
sudo dpkg -i percona-release_latest.$(lsb_release -sc)_all.deb
sudo percona-release setup ps80
sudo apt-get install percona-server-client
sudo apt-get install libmysqlclient-dev
```

# Create MySQL DB user

```bash
mysql -u root -p
CREATE DATABASE cybros_vendor character set UTF8mb4 collate utf8mb4_bin;
CREATE USER 'cybros_vendor'@'%' IDENTIFIED BY 'cybros_vendor_password';
GRANT ALL ON cybros_vendor.* TO 'cybros_vendor'@'%';
FLUSH PRIVILEGES;
```
