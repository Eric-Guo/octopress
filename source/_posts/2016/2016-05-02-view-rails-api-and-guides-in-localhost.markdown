---
layout: post
title: "View Rails API and Guides in localhost"
date: 2016-05-02T00:02:38+08:00
comments: true
external-url: 
categories: [Ruby, Rails]
---

Dash 3 switch to the subscription mode, if you just want to see the Rails document, in fact, it's relative easier to build in your own box.

## Building Rails API Document

```
cd ~/git/oss/
git clone https://github.com/rails/rails
cd rails/
bundler install
rake rdoc
```

## Building Rails Guides

```
cd guides/
rake guides:generate:html
```

## Enable Apache

```bash
sudo apachectl start
```

## Link Rails documents

```
cd /Library/WebServer/Documents/
sudo ln -s /Users/guochunzhong/git/oss/rails/doc/rdoc/ rails_api
sudo ln -s /Users/guochunzhong/git/oss/rails/guides/output/ rails_guides
```

## View in localhost

visit Rails API at [http://localhost/rails_api/](http://localhost/rails_api/)

visit Rails Guides at [http://localhost/rails_guides/](http://localhost/rails_guides/)