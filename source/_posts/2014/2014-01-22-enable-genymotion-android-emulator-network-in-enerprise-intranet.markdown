---
layout: post
title: "Enable Genymotion Android Emulator network in Intranet"
date: 2014-01-22T11:14:47+08:00
comments: true
external-url:
categories: [Android]
---

[Genymotion](http://www.genymotion.com/) is very fast compare to original android simulator, but make it work in Intranet takes me quite a lot of time.

In fact, it's simple: Open the Oracle VM VirtualBox manager, Settings->Network, Select the Adapter 2 (Adapter 1 is used primary by Genymotion and cannot change), using Bridged Adapter and select the working network card in Intranet (not wireless usually).