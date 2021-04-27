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

# Do the real deploy

```bash
bundle exec cap production deploy
```

# New nginx conf

```nginx Sample nginx configure file
server {
  server_name coreui.redwoodjs.cn;
  index index.html;
  root /var/www/coreui4_demo/current/dist;

  location / {
    try_files $uri $uri/ /index.html;
    access_log /var/www/coreui4_demo/shared/log/nginx.access.log;
    error_log /var/www/coreui4_demo/shared/log/nginx.error.log;
  }

  listen 443 ssl; # managed by Certbot
  ssl_certificate /etc/letsencrypt/live/api.wefocusin.com/fullchain.pem; # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/api.wefocusin.com/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
  if ($host = coreui.redwoodjs.cn) {
      return 301 https://$host$request_uri;
  } # managed by Certbot

  listen 80;
  server_name coreui.redwoodjs.cn;
  return 404; # managed by Certbot
}
```
