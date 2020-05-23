---
layout: post
title: "Install snell-server in CentOS 7"
date: 2020-03-18T08:10:32+08:00
comments: true
external-url:
categories: [Surge, Snell]
---

## Prepare the execution files & account

```bash
adduser snell
sudo su - snell
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
wget https://github.com/surge-networks/snell/releases/download/v2.0.2/snell-server-v2.0.2-linux-amd64.zip
unzip snell-server-v2.0.2-linux-amd64.zip
mkdir snell
mv snell-server snell
rm snell-server-v2.0.2-linux-amd64.zip
cd snell
./snell-server # generate conf file
```

## Configure the system auto start

```bash
vi /etc/systemd/system/snell.service
```

```text
[Unit]
Description=Snell Proxy Service
After=network.target

[Service]
Type=simple
User=snell
Group=snell
LimitNOFILE=32768
ExecStart=/home/snell/snell/snell-server -c /home/snell/snell/snell-server.conf

[Install]
WantedBy=multi-user.target
```

## Restart or run below

```bash
systemctl daemon-reload
systemctl start snell
systemctl restart snell
systemctl enable snell
cat /home/snell/snell/snell-server.conf
```
