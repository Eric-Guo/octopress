---
layout: post
title: "A true story about upgrade Crystal Report from Windows 2003 to Windows 2008"
date: 2012-01-29 16:18
comments: true
external-url:
categories: [Crystal Report, Windows7]
---
I written a post about <a href="/2011/12/18/true-story-about-upgrade-crystal-report-from-oracle-9i-to-11g">upgrade CR (Crystal Report) from Oracle 9i to Oracle 11gR2</a> one month ago, at that post, I talked about how different version of Oracle Database make Crystal Report migrate tough/broken and how I have solving those problem. After the Crystal Report Travel Card (T-Card) online this month, I also meet some problem relative with the Windows Server, which I think also very interesting and hoping you will enjoy the story.

<!--more-->

After make sure all the T-Card printed properly in the test environment, I start install the whole MES Travel Card printing system on the production which will be used on 1/2/2012 to our production environment. There is hundreds of printer; user can use any of the printers which are near him to output the paper Travel Card in the MES system. During the UAT, user tested some T-Card to output to their printer and suddenly I found that production environment T-Card printing system can only output the T-Card in test printer.

Certainly I enable the printing system logs to trace the problem immediately and found&hellip; there is nothing wrong in the log. The printing system just block at setting printer in the log:

```text
[00:19:05]: [L3][R0]Transacting to report printer
[00:19:05]: [L5][R0] Start Loading report template: D:\Documents\TravelCards\TC-SKU.rpt
[00:19:54]: [L5][R0] Setting report Logon
[00:19:55]: [L5][R0] Collecting report parameters
[00:19:55]: [L5][R0] Param: Lot Number found in report. Passing in value: M1202T1D1C
[00:19:55]: [L5][R0] Setting printer to: \\CVPWFSIP03\CVPPRT55
```

There is nothing reported after that and the program just dead like a dummy, feeling very frustrated I start to write a command line based test program &ndash; <em>PrintingSystemTestApp.exe</em> and hoping can found something, now I'm lucky and I got it:

{% img /images/2012/need_confirm_dialog_when_install_printer_drivers.png Windows Server 2008 need confirm in dialog by default before install a printer driver %}

So that's the point: The printer in Windows 2008 need manually install its printer driver once before it can working properly in windows service program. A secured windows server will make printing function not work at all in production!

So I just write to the our company's windows team to see if those manually confirm dialog can be by passed similar to UAC control (Computer Configuration -&gt; Windows Setting -&gt; Security Settings -&gt; Local Policies -&gt; Security Options -&gt; Elevate without prompting) at Group Policy.

The answer is yes! Based on the <a href="http://technet.microsoft.com/en-us/library/cc753269.aspx" target="_blank">http://technet.microsoft.com/en-us/library/cc753269.aspx</a><span style="color: #1f497d;"> </span>TechNet document, here is the Group Policy access path: Local Computer Policy -&gt; User Configuration -&gt; Administrative Templates -&gt; Control Panel -&gt; Printers -&gt; <strong>Point and Print Restriction:</strong>

{% img /images/2012/point_print_restriction_group_policy_configure_dialog.png point & print restriction group policy configure dialog %}

After disable this feature, the production T-Card printed properly now and all user are now happy!