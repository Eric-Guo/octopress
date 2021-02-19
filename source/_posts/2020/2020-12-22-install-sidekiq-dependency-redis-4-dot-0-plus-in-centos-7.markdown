---
layout: post
title: "Install sidekiq dependency redis 4.0+ in CentOS 7"
date: 2020-12-22T12:46:04+08:00
comments: true
external-url: 
categories: [CentOS, Ruby]
---

[Sidekiq 6.0 require Redis 4.0+](https://github.com/mperham/sidekiq/blob/master/6.0-Upgrade.md), which the default redis version in CentOS still 3.x, so here is how to install redis 4.0+ in CentOS 7

```bash
yum install -y http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum --enablerepo=remi install redis
systemctl start redis
redis-cli --version
systemctl enable redis.service
vi /etc/redis.conf # if require
```
