---
layout: post
title: "Call brew install Ruby from Mathematica Wolfram Kernel"
date: 2021-06-13T11:03:51+08:00
comments: true
external-url: 
categories: [Wolfram, Mathematica]
---


```nb
RegisterExternalEvaluator["Ruby", "/usr/local/opt/ruby/bin/ruby"]
ExternalEvaluate["Ruby", "RUBY_VERSION"]
```

It's works even exit in wolfram kernel.
