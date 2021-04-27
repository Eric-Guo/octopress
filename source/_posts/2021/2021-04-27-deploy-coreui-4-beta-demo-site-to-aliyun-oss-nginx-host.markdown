---
layout: post
title: "Deploy coreui 4 beta demo site to Aliyun OSS nginx host"
date: 2021-04-27T14:12:55+08:00
comments: true
external-url: 
categories: [Bootstrap, CoreUI]
---

Due to Core UI 4 still no demo site, but I already want to using it, so I built a site myself.

# Add a dedicated user

```bash
adduser coreui4_demo
sudo su - coreui4_demo
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys # and paste your public key
chmod 600 .ssh/authorized_keys
```

# Prepare deploy folder

```bash
cd /var/www
mkdir coreui4_demo
chown coreui4_demo:coreui4_demo coreui4_demo
```

# Change code to deploy

See [cn_site](https://github.com/Eric-Guo/coreui-free-bootstrap-admin-template/commits/cn_site) branch for detail modifies.
