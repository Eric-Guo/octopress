---
layout: post
title: "rbenv and CentOS Ruby on Rails production environment install log"
date: 2018-08-19T22:51:11+08:00
comments: true
external-url:
categories:
---

[Original refer](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-centos-7), install on a Aliyun server.

# Install software in root account

## Update system

```bash
yum update
yum install -y htop git zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
reboot
```

# Setup a user account

```bash
adduser deployer
gpasswd -a deployer wheel
visudo # add deployer ALL=(ALL) NOPASSWD: ALL at end
sudo su - deployer
mkdir .ssh
chmod 700 .ssh
```

Also disable root login and password via `PermitRootLogin` in `/etc/ssh/sshd_config`

Before exis, make sure you can login via `ssh deployer@ip_address`, other wise, [check file permission](https://unix.stackexchange.com/a/36687/303385).

# Install rbenv and ruby-build

```bash
cd # as a deployer
git clone git://github.com/sstephenson/rbenv.git .rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
~/.rbenv/bin/rbenv init
# As an rbenv plugin
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

# Install Ruby 2.5.1

```bash
rbenv install -l
rbenv install 2.5.1
rbenv global 2.5.1
eval "$(rbenv init -)" >> ~/.bash_profile
echo "gem: --no-document" > ~/.gemrc
gem install bundler
```

# Install Javascript Runtime

```bash
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
sudo yum install nodejs
sudo curl -sL https://dl.yarnpkg.com/rpm/yarn.repo -o /etc/yum.repos.d/yarn.repo
sudo yum install yarn
```

# [Install postgresql](https://www.linode.com/docs/databases/postgresql/how-to-install-postgresql-relational-databases-on-centos-7/)

```
sudo yum install postgresql-server postgresql-contrib postgresql-devel
sudo postgresql-setup initdb
sudo systemctl start postgresql
```

```
createuser deployer --pwprompt
ALTER ROLE deployer LOGIN
CREATE DATABASE harman_vendor_production WITH ENCODING='UTF8' OWNER=deployer;
```

```bash /var/lib/pgsql/data/pg_hba.conf
# "local" is for Unix domain socket connections only
local    all        all             peer
```

```bash
psql -d harman_vendor_production
```

# Install nginx

```bash
sudo yum install epel-release
sudo yum install nginx
```

# Fix permission for CentOS

```bash
sudo mkdir /var/www
cd /var/www
sudo mkdir jbl_product
sudo chown deployer:deployer jbl_product/
```

or further read [nginx permission denied](https://stackoverflow.com/questions/23948527/13-permission-denied-while-connecting-to-upstreamnginx)
