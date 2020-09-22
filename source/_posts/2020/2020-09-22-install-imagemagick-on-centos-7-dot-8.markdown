---
layout: post
title: "Install ImageMagick 7.0.10 on CentOS 7.8"
date: 2020-09-22T16:37:07+08:00
comments: true
external-url:
categories:
---

Go official imageMagick website to [download](https://imagemagick.org/script/download.php)


```bash
sudo su -
wget https://imagemagick.org/download/linux/CentOS/x86_64/ImageMagick-libs-7.0.10-30.x86_64.rpm
wget https://imagemagick.org/download/linux/CentOS/x86_64/ImageMagick-7.0.10-30.x86_64.rpm
yum install libraqm
yum install fftw
rpm -Uvh ImageMagick-libs-7.0.10-30.x86_64.rpm
yum remove ImageMagick #  if 6.9.10.68 exist
rpm -Uvh ImageMagick-7.0.10-30.x86_64.rpm
```
