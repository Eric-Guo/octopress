---
layout: post
title: "Invisible characters cause Excel vlookup failure"
date: 2014-06-19T13:44:37+08:00
comments: true
external-url:
categories:
---

After 30 minutes to solve invisible char cause Excel vlookup failed,  I find below fomula is useful:

```
=VLOOKUP(I2&"*",Sheet2!C:C,1,0)
```