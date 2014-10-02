---
layout: post
title: "Checking gem reverse dependencies"
date: 2014-10-02T09:58:38+08:00
comments: true
external-url:
categories: [Ruby]
---

The tip is coming from [Faria DevTips](http://devtips.faria.co/2014-05-27-gem-reverse-dependencies.html).

```bash
ruby -ropen-uri -rpp -ryaml -e 'pp YAML.load(open("https://rubygems.org/api/v1/gems/parser/reverse_dependencies.yaml"))'
```