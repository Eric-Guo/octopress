---
layout: post
title: "One line to pack individual PDF file into their BaseName.RAR in PowerShell"
date: 2011-10-17 15:01
comments: true
external-url:
categories: [PowerShell]
---
```powershell
gci *.pdf | % {iex $("rar mf -m5 -rr1 {0}.rar '{1}'" -f $($_.BaseName -replace "\s+","."),$_.Name)}
```

<!--more-->

Here is some explain:

* gci is Alias of Get-ChildItem
* iex is Alias of Invoke-Expression
* rar is the execution file which default locate at C:\Program Files\WinRAR