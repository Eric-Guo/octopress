---
layout: post
title: "Install Ruby on Rails production based on Amazon Linux 2 AMI (HVM)"
date: 2019-03-11T11:29:30+08:00
comments: true
external-url:
categories: [Amazon]
---

Based my last [rbenv and CentOS Ruby on Rails production environment install log](/2018/08/19/rbenv-and-centos-ruby-on-rails-production-environment-install-log/), but this time on AWS!

But will only record the difference as below:

Run `sudo amazon-linux-extras install nginx1.12` instead of `sudo yum install nginx` to install nginx.

Seems AMI provide their own version of below package.

```
  0  ansible2                 available    [ =2.4.2  =2.4.6 ]
  2  httpd_modules            available    [ =1.0 ]
  3  memcached1.5             available    [ =1.5.1 ]
  4  nginx1.12=latest         enabled      [ =1.12.2 ]
  5  postgresql9.6            available    [ =9.6.6  =9.6.8 ]
  6  postgresql10             available    [ =10 ]
  8  redis4.0                 available    [ =4.0.5  =4.0.10 ]
  9  R3.4                     available    [ =3.4.3 ]
 10  rust1                    available    \
        [ =1.22.1  =1.26.0  =1.26.1  =1.27.2  =1.31.0 ]
 11  vim                      available    [ =8.0 ]
 13  ruby2.4                  available    [ =2.4.2  =2.4.4 ]
 15  php7.2                   available    \
        [ =7.2.0  =7.2.4  =7.2.5  =7.2.8  =7.2.11  =7.2.13  =7.2.14 ]
 16  php7.1                   available    [ =7.1.22  =7.1.25 ]
 17  lamp-mariadb10.2-php7.2  available    \
        [ =10.2.10_7.2.0  =10.2.10_7.2.4  =10.2.10_7.2.5
          =10.2.10_7.2.8  =10.2.10_7.2.11  =10.2.10_7.2.13
          =10.2.10_7.2.14 ]
 18  libreoffice              available    [ =5.0.6.2_15  =5.3.6.1 ]
 19  gimp                     available    [ =2.8.22 ]
 20  docker=latest            enabled      \
        [ =17.12.1  =18.03.1  =18.06.1 ]
 21  mate-desktop1.x          available    [ =1.19.0  =1.20.0 ]
 22  GraphicsMagick1.3        available    [ =1.3.29 ]
 23  tomcat8.5                available    \
        [ =8.5.31  =8.5.32  =8.5.38 ]
 24  epel                     available    [ =7.11 ]
 25  testing                  available    [ =1.0 ]
 26  ecs                      available    [ =stable ]
 27  corretto8                available    [ =1.8.0_192  =1.8.0_202 ]
 28  firecracker              available    [ =0.11 ]
 29  golang1.11               available    [ =1.11.3 ]
 30  squid4                   available    [ =4 ]
 31  php7.3                   available    [ =7.3.2 ]
 32  lustre2.10               available    [ =2.10.5 ]
```
