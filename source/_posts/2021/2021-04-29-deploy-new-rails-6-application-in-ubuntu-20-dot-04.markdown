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

# Enable new user as sudo

```bash
sudo su -
cd /etc/sudoers.d/
echo "cybros_vendor ALL=(ALL) NOPASSWD:ALL" > 80-cybros_vendor-user
```

# Install nginx

```bash
sudo apt-get install nginx
```

It's also possible to [enable TLS 1.1/1.2 due to Ubuntu 20.04 not support by default](https://askubuntu.com/questions/1240595/enable-tls-1-0-and-tls-1-1-on-ubuntu-20-04).

```nginx /etc/nginx/nginx.conf
ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
# seclevel for TLS 1.0 and 1.1
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH:@SECLEVEL=1";
```

# Install node.js 15 and yarn 1.x

Using [nodesource distribution](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)

```bash
curl -fsSL https://deb.nodesource.com/setup_15.x | sudo -E bash -
sudo apt-get install gcc g++ make git
sudo apt-get install -y nodejs
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

# Link rbenv to make capistrano works

```bash
mkdir -p ~/.rbenv/bin
cd ~/.rbenv/bin
ln -s /usr/bin/rbenv rbenv
```

# Install Oracle Instant Client

[Download Version 19.11.0.0.0](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html) and following [ruby-oci8 document](https://github.com/kubo/ruby-oci8/blob/master/docs/install-instant-client.md#unix-zip-packages)

```bash
sudo mkdir /opt/oracle
sudo unzip instantclient-basic-linux.x64-19.11.0.0.0dbru.zip -d /opt/oracle
sudo unzip instantclient-sqlplus-linux.x64-19.11.0.0.0dbru.zip -d /opt/oracle/
sudo unzip instantclient-sdk-linux.x64-19.11.0.0.0dbru.zip -d /opt/oracle/
cd /usr/local/bin
sudo ln -s /opt/oracle/instantclient_19_11/sqlplus  sqlplus
export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_11
gem install ruby-oci8
```

```text Append to /etc/environment
TNS_ADMIN=/opt/oracle/instantclient_19_11/network/admin
LD_LIBRARY_PATH=/opt/oracle/instantclient_19_11
NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```

Also need setting in `systemd/system/puma_service` or `side_service`

```text
Environment="TNS_ADMIN=/opt/oracle/instantclient_19_11/network/admin"
Environment="LD_LIBRARY_PATH=/opt/oracle/instantclient_19_11"
Environment="NLS_LANG=AMERICAN_AMERICA.AL32UTF8"
```
