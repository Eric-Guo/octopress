---
layout: post
title: "How to install Windows 7 on the 6 years IBM X60 notebook"
date: 2011-09-15 14:29
comments: true
external-url:
categories: [Windows7, eDevice]
---
IBM X60 is very legacy PC, very light and have active hard-drive protection so X60 is quite reliable. but due to its hard drive AHCI mode and no CD-ROM, it is really a nightmare when you plan to re-install the windows.

But fortunately, you can use the USB disk to install the Windows 7 very easily and fast (no driver problem in windows 7 for X60), so the key problem for install the Windows 7 on X60 become how to build a bootable Windows 7 installation media on USB disk and how to successfully boot it in the X60 when startup.

<!--more-->

First problem: you can use the UltraISO version 9.3+ version, select Bootable-&gt;Write Disk Image from the menu, using write method USB-ZIP+ to build a bootable and workable Windows 7 installation media.

Second problem: press ThinkAdvantage blue key to enter the BIOS setup UI, Config-&gt;Hard Driver-&gt;switch to ACHI interface and using enable &ndash;USB-HDD boot item in the startup menu, save and exit.

Then you will enjoy the most up-to-date Windows 7 experience on the 6 years IBM X60 now, there is no drivers installation headache in win7.