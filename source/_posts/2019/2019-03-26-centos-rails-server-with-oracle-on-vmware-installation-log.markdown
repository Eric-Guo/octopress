---
layout: post
title: "CentOS Rails server with Oracle on VMware Installation log"
date: 2019-03-26T11:23:14+08:00
comments: true
external-url:
categories: [CentOS]
---


First CentOS server installed in VMware as [thape](https://github.com/thape-cn/oauth2id) SSO server.

# Install software in root account

## Update system

Run as root:

```bash
yum update
yum install -y git zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -ivh epel-release-latest-7.noarch.rpm
yum --enablerepo=epel install htop
reboot
```

As VMware admin required, not disable the `PermitRootLogin` in `/etc/ssh/sshd_config`

# Setup a user account

```bash
adduser deployer
gpasswd -a deployer wheel
visudo # add deployer ALL=(ALL) NOPASSWD: ALL at end
sudo su - deployer
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

Also disable root login and password via `PermitRootLogin` in `/etc/ssh/sshd_config`

Before exis, make sure you can login via `ssh deployer@ip_address`, other wise, [check file permission](https://unix.stackexchange.com/a/36687/303385).

# Install rbenv and ruby-build

```bash
cd # as a deployer
git clone https://github.com/sstephenson/rbenv.git .rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
~/.rbenv/bin/rbenv init
# As an rbenv plugin
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
```

# Install Ruby 2.6.5

```bash
rbenv install -l
rbenv install 2.6.5
rbenv global 2.6.5
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
echo "gem: --no-document" > ~/.gemrc
gem install bundler
gem install bundler -v 1.17.3
```

# Install Javascript Runtime

Run as root:

```bash
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
sudo yum install nodejs
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
sudo yum install yarn
# if you behind GFW
npm config set registry https://registry.npm.taobao.org/ --global
npm config set disturl https://npm.taobao.org/dist --global
yarn config set registry https://registry.npm.taobao.org/ --global
yarn config set disturl https://npm.taobao.org/dist --global
```

# Install nginx

```bash
sudo yum install epel-release
sudo yum install nginx
chkconfig nginx on
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload
```

# Fix permission for CentOS

```bash
sudo mkdir /var/www
cd /var/www
sudo mkdir cybros
sudo chown deployer:deployer cybros/
```

and disable selinux(https://linuxize.com/post/how-to-disable-selinux-on-centos-7/),

or further read [nginx permission denied](https://stackoverflow.com/questions/23948527/13-permission-denied-while-connecting-to-upstreamnginx)

# Install Oracle Instant Client

[Download Version 12.2.0.1.0](https://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html) and following [ruby-oci8 document](https://github.com/kubo/ruby-oci8/blob/master/docs/install-instant-client.md#install-oracle-instant-client-packages)

```bash
sudo rpm -i ./oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm
sudo rpm -i ./oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm
sudo rpm -i ./oracle-instantclient12.2-sqlplus-12.2.0.1.0-1.x86_64.rpm
cd /usr/local/bin
sudo ln -s /usr/bin/sqlplus64 sqlplus
export LD_LIBRARY_PATH=/usr/lib/oracle/12.2/client64/lib
gem install ruby-oci8
```

```bash Append to ~/.bashrc
export LD_LIBRARY_PATH=/usr/lib/oracle/12.2/client64/lib
export NLS_LANG=en_US.UTF-8
```

# Install FreeTDS to connect to SQL Server

```bash
sudo su -
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.111.tar.gz
tar -xzf freetds-1.00.111.tar.gz
cd freetds-1.00.111
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
logout # as deployer
gem install tiny_tds
```

# Install MySQL

[Install the percona server via yum](https://www.percona.com/doc/percona-server/5.7/installation/yum_repo.html).

After install, [do the secure installation](https://stackoverflow.com/questions/36028166/can-not-connect-to-mysql-server-percona) for root.

```bash
mysql -u root -p
CREATE DATABASE cybros_prod character set UTF8mb4 collate utf8mb4_bin;
CREATE USER 'cybros_prod'@'localhost' IDENTIFIED BY 'new_password';
GRANT ALL ON cybros_prod.* TO 'cybros_prod'@'localhost';
FLUSH PRIVILEGES;
```
