---
layout: post
title: "Deploy TARO documents to the 4 GB memory server"
date: 2021-06-23T09:37:07+08:00
comments: true
external-url: 
categories: [Taro, Linux]
---

The official taro document build setting `NODE_OPTIONS=--max-old-space-size=5120` in packages.json, but I would link to deploy a TARO documents to my 4 GB memory server, so here is how to.

# Change NODE_OPTIONS settings

Change to `NODE_OPTIONS=--max-old-space-size=2500` is reasonable in a 4GB memory server, but that's far from enough.

# Enable the swap

```bash
sudo fallocate -l 1G /swapfile
# or sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
# To make it permanent
sudo echo '/swapfile swap swap defaults 0 0' >> /etc/fstab
```

# Stop more services while building

```bash
sudo systemctl stop postgresql-12
sudo systemctl stop memcached
sudo systemctl stop nginx
sudo systemctl stop docker
```

It can be restart after reboot.

# Install lib vips

The `@docusaurus/core` like `2.0.0-beta.1` require vips library, so need [install vips at CentOS 7](https://tufora.com/tutorials/linux/general/install-vips-vips-tools-and-vips-devel-libvips-on-centos-7)

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install yum-utils
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
sudo yum-config-manager --enable remi
sudo yum install vips vips-devel vips-tools
```
