---
layout: post
title: "Access Windows Update by Microsoft Site in a domain policy disable environment"
date: 2011-11-10 15:08
comments: true
external-url:
categories: [Windows7, Tips]
---
A lot of company windows domain system administrator will disable user to access Windows Update in Microsoft Site and push the patch to your desktop via a internal site, but for the developer who using tools like Visual Studio or SQL Server, the domain system administrator won't help you.

<!--more-->

So the below content will help you, you can copy and save as "enable_windows_update.reg" file and double click to enable the Windows Update:

```ini
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\WindowsUpdate]
"DisableWindowsUpdateAccess"=dword:00000000

[HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Policies\WindowsUpdate]
"DisableWindowsUpdateAccess"=dword:00000000
```