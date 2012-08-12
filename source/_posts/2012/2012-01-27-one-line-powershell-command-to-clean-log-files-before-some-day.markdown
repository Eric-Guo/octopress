---
layout: post
title: "One line PowerShell command to clean log files before some day"
date: 2012-01-27 16:09
comments: true
external-url:
categories: [PowerShell]
---
```powershell
icm -ScriptBlock {gci d:\Temp\*.txt | ?{$_.LastWriteTime -lt [datetime]"3/24/2012"} | ri } -ComputerName cvpmesip01,cvpmesip02,cvpmesip03,cvpmesip04
```
Like similar Unix one line command, the command usually need more text content to explain, <!--more-->the function above script will auto clean the log file earlier than 1/27/2012 on the computer cvpmesip01~04, the four computers should already preconfigured the Windows Remote Management (usually just by run winrm quickconfig in that server command prompt), you should also an administrator for those server and using AD. After those prerequisite are all meet, icm (Invoke-Command) can launch any ScriptBlock on any windows server now.

The actual working command is ri(Remove-Item), it filter by ? (Where-Object) and listed by gci(Get-ChildItem), all three is basic PowerShell Cmdlet which pipe line by "|", <strong>the biggest difference between PowerShell and Unix is the pipe line is by object instead of text string</strong>, so it is possible to compare the file attribute LastWriteTime, the <strong>$_</strong> is automatic variable, contains the current object in the pipeline object. You can use this variable in commands that perform an action on every object or on selected objects in a pipeline.