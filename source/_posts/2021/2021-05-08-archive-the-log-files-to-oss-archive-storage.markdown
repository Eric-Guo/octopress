---
layout: post
title: "Archive the log files to OSS archive storage"
date: 2021-05-08T16:47:06+08:00
comments: true
external-url: 
categories: [Aliyun]
---

Notice [Region ID](https://help.aliyun.com/document_detail/31837.html) when configure the `aliyun` cli, should write as `cn-shanghai` or `cn-shanghai-internal`

And below command is upload all files in current folder to `thape_web_gz`.

```bash
aliyun oss cp . oss://thape-web-log/thape_web_gz -r
```
