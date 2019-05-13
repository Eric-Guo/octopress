---
layout: post
title: "Using Aliyun OSS as CDN for the Rails application"
date: 2019-05-13T16:19:25+08:00
comments: true
external-url:
categories: [Rails]
---

用Rails做企业官网虽然不是现在的流行方式，但是考虑到Rails的灵活性和后端开发的方便性，在某些情况下，还是相比现在最火的[JAM gatsby.js](https://www.gatsbyjs.org/blog/2018-02-16-bright-future-for-the-web/)更实际。

企业官网一般图片等静态资源非常多，但是用Rails的话，由于很低的访问量，去购买一台高带宽的服务器又很不划算，所幸的是，阿里云的OSS提供了回源，通过适当的配置，就可以将那些大图片，大字体移到按流量付费的OSS上，获得极大的速度提升，基本原理如下：


```text
                      [User]
                         |
<https://thape-assets.oss-cn-shanghai.aliyuncs.com/assets/application-digest.js>
                         |
               ---------------------------------------
               |                                     |
            <cache>                             <no cache>
               |                                     |
             [200]                <https://www.thape.com.cn/assets/application-digest.js>
                                                     |
                                           [Nginx location /assets]
                                                     |
                                                   [200] --> [CDN Cache]
```

配置方法也非常简单，新建一个OSS Bucket，例如上图中的名字`thape-assets`

* 读写权限为`公共读`
* 防盗链，Referer加入 https://www.thape.com.cn/ 和 https://thape-assets.oss-cn-shanghai.aliyuncs.com/ ，可允许空Referer
* 跨域规则，来源：https://www.thape.com.cn ，允许方法：GET和HEAD，允许Header为 * ，缓存时间 60 秒
* 镜像回源，回源类型设置为镜像，回源条件为404时，文件名前缀：`assets/`，回源地址为 https://www.thape.com.cn/
* 生命周期，对整个 Bucket：60天后删除文件，30天后删除碎片


最后在Rails中的 `config/environments/production.rb` 中，启用新的OSS地址即可。

```ruby
  # Enable serving of images, stylesheets, and JavaScripts from an asset server.
  config.action_controller.asset_host = 'https://thape-assets.oss-cn-shanghai.aliyuncs.com'
```
