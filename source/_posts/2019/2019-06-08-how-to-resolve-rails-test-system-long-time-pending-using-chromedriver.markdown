---
layout: post
title: "How to resolve rails test:system long time pending using chromedriver"
date: 2019-06-08T19:05:00+08:00
comments: true
external-url:
categories: [Rails, Chrome]
---

Running `rails test:system` in my local dev always pending and timeout problem haunted me for a long time, finally, I know why.

Due to some change introduced in Google, every time a new chromedriver installed using brew, the first time running always need to download a file from `storage.l.googleusercontent.com`. but due to there is GFW in China, it always pending.

So resolved is quite simple, make sure you running `ruby.exe`(or binary of ruby) when first time can access google site and that's it.

You need to make sure only once per every new version, that's it!
