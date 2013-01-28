---
layout: post
title: "Free the Windows 7 hard drive space safely by Space Sniffer"
date: 2012-02-23 16:51
comments: true
external-url:
categories: [Tips, Windows7]
---
My Windows 7 DELL D630 is two year old now, I try installed quite a dozens of software, running a lot of work job, programming several program language and using IDE/reporting on it and I'm satisfy with Windows 7, except now my C drive space is only 1GB left. The D630 Windows 7 C drive has totally 57GB, I known it is very cheap in today's hard drive and it is even no need to check the hard drive if your time is more valuable than the space cost, but I use the Sandisk SSD which is very fast but total capacity is only 120GB and I'm already in the situation that the Windows 7 OS will soon out of work due to free space exhausting.

Which is my best choose? Re-installing the Windows 7 will take 3 days. (yes, adjust the hundreds of setting to my favorate needs and install 3 DB, 2 IDE, 2 Reporting Application, etc really takes that long time.) Deleting some of the installed application almost no use because I need to use it daily so I install it and I need those application. Running the windows cleanup can only clean very little space and it is also helpless in this case.

<!--more-->

That's why I like the <a href="http://spacesniffer.en.softonic.com/" target="_blank">Space Sniffer</a>, the graph hard drive occupation presentation tools, which can easily help me answer the question: Where is my hard drive space gone? Here is my C drive map:

{% img /images/2012/my_dell_D630_c_drive.png Space Sniffer show Dell D630 C drive occupation %}

I soon find the Symantec Antivirus Live Update takes me 895MB and according to the Symantec <a href="http://www.symantec.com/connect/forums/change-liveupdate-download-directory" target="_blank">Technical Specialist Suggested</a>: &ldquo;these zip files can be deleted from the download folder, there is no harm in it&rdquo; the folder: C:\ProgramData\Symantec\LiveUpdate\Downloads can be safely totally cleaned by Control Panel-&gt;Live Updates. Another place which occupies 700MB space is C:\Users\6749\AppData\Roaming\115\Box\UserData\4035931\Error.log, which is only log but takes 700MB!

After I delete those files I free 1.5GB space and now my Windows 7 is back to normal now with 3.2 GB free, it's great.