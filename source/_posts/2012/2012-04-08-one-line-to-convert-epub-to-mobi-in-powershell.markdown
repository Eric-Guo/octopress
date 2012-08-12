---
layout: post
title: "One line to convert ePub file into the mobi file in PowerShell"
date: 2012-04-8 17:45
comments: true
external-url:
categories: [PowerShell]
---
```powershell
gci *.epub | % {iex $("kindlegen {1} -c2 -o '{0}.mobi'" -f $($_.BaseName -replace "\."," "),$_.Name)}
```