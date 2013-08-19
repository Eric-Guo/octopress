---
layout: post
title: "Enable directory listing in Nginx"
date: 2013-08-19T10:53:47+08:00
comments: true
external-url:
categories: [Nginx]
---

Step 1, setting [the autoindex on in conf](http://nginxlibrary.com/enable-directory-listing/)

```nginx
server {
  listen       80;
  server_name  cvpforms; #replace your DNS name
  server_name  cvpforms.sandisk.com;
  rails_env    development; #development/production
  root         /var/rails_apps/pl-form/public;
  passenger_enabled on;
  client_max_body_size 5m;
  location /rf_image {
    autoindex on;
    autoindex_exact_size off;
  }
}
```

Step 2, [resolve the 403 forbidden error](http://www.nginxtips.com/403-forbidden-nginx/) using below command if you meet such error.

```bash
chmod 755 * -R # in /home/pl-form/pl-form/public/rf_image
```