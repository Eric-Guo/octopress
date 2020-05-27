---
layout: post
title: "ignore or skip the macOS 10.15.5 Catalina update in macOS 10.14.6 (18G5033) Mojave"
date: 2020-05-27T08:37:12+08:00
comments: true
external-url:
categories:
---

```bash
sudo softwareupdate --ignore "macOS Catalina"
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
killall Dock
```

But the sad thing is you can not using this methods after 10.15.5, which add another big reason *not* upgrade to macOS 10.15.5 Catalina.


