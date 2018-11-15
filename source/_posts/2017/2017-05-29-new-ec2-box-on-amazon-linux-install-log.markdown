---
layout: post
title: "New EC2 box on Amazon Linux install log"
date: 2017-05-29T15:55:22+08:00
comments: true
external-url:
categories: [AWS, Linux]
---


## Install postgresql 9.6

```bash
sudo yum update
# https://yum.postgresql.org/repopackages.php#pg96
sudo yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-6-x86_64/pgdg-ami201503-96-9.6-2.noarch.rpm
sudo yum install -y postgresql96 postgresql96-server postgresql96-libs postgresql96-contrib
sudo /etc/init.d/postgresql-9.6 initdb
sudo /etc/init.d/postgresql-9.6 start
sudo chkconfig postgresql-9.6 on
sudo -u postgres -i psql -c 'SELECT version();'
```

## Install nginx

```bash
sudo yum install -y nginx
sudo chkconfig nginx on
```

## Install redis

See https://medium.com/@andrewcbass/install-redis-v3-2-on-aws-ec2-instance-93259d40a3ce

## Install node.js

```bash
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
yum install -y gcc-c++ make
yum install -y nodejs
```

## Install Go

```bash
wget https://storage.googleapis.com/golang/go1.11.2.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz
echo "export PATH=$PATH:/usr/local/go/bin" > /etc/profile.d/go-lang.sh
```

Notes, if you upgrade new version, need `rm -rf /user/local/go` first.

## Install shadowsocks-go

```bash
sudo yum install -y git
go get github.com/shadowsocks/shadowsocks-go/cmd/shadowsocks-server
wget -4qO- "http://whatismyip.akamai.com/" # record public IP
```

Build [config.json](https://github.com/shadowsocks/shadowsocks-go/blob/master/config.json)
```json
{
    "server":"127.0.0.1",
    "server_port":8388,
    "local_port":1080,
    "password":"barfoo!",
    "method": "aes-128-cfb-auth",
    "timeout":600
}
```

```bash
cd ~/go/bin && nohup ./shadowsocks-server &
```

## Install Mosh

```bash
sudo yum --enablerepo=epel install -y mosh
```
