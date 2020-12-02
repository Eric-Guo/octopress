---
layout: post
title: "Migrate capistrano3-puma to using SystemD mode and puma 5.0"
date: 2020-12-02T09:53:33+08:00
comments: true
external-url: 
categories: [capistrano, puma]
---

## Source code change

Puma 5.0 [removed eaemonization without replacement](https://github.com/puma/puma/blob/master/History.md#500--2020-09-17) and finally capistrano3-puma start using systemd to manage the puma server and here is need to do to upgrade puma 4 to puma 5.

```diff Capfile
require 'capistrano/puma'
install_plugin Capistrano::Puma
install_plugin Capistrano::Puma::Nginx
+install_plugin Capistrano::Puma::Systemd
```

## Enable deployer user sudo.

```bash
sudo su -
cd /etc/sudoers.d/
echo 'deployer ALL=(ALL) NOPASSWD:ALL' > 80-deployer-user
visudo # refresh only
```

## Enable systemd puma.service

```bash
sudo mv /etc/systemd/system/puma.service /etc/systemd/system/puma_prod.service
# $get home folder like /home/deployer
cd && pwd
# replace $home with /home/deployer
sudo vi /etc/systemd/system/puma_prod.service
sudo systemctl daemon-reload
sudo systemctl enable puma_prod
```

## NLS_LANG setting for systemd

Modify systemd.conf at `/etc/systemd/system.conf`

```text system.conf
DefaultEnvironment="NLS_LANG='AMERICAN_AMERICA.AL32UTF8'"
```
