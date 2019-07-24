---
layout: post
title: "IE 11 iframe weird X-Frame-Options but working setting"
date: 2019-07-24T09:08:58+08:00
comments: true
external-url:
categories: [Rails, Windows]
---


After research 1 hours, I answered in the [SO](https://stackoverflow.com/questions/38837388/internet-explorer-x-frame-options-allow-from-not-working-in-ie-11-and-edge/57173993#57173993)

```ruby
def cors_set_access_control_headers
  headers["Access-Control-Allow-Origin"] = "*"
  headers["Access-Control-Allow-Methods"] = "GET"
  headers["Access-Control-Request-Method"] = "*"
  headers["Access-Control-Allow-Headers"] = "Origin, X-Requested-With, Content-Type, Accept, Authorization"
  headers["X-Frame-Options"] = "ALLOW-FROM http://172.16.1.159"
  headers["X-XSS-Protection"] = "0"
end
```
