---
layout: post
title: "Running Knoppix 6.7.1 in a Sandisk Ultra flash drive to do GAE programming via PyCharm"
date: 2011-10-04 14:48
comments: true
external-url:
categories: [Linux, eDevice, Knoppix]
---
A couple of days ago, I just <a href="/2011/09/15/how-to-install-windows-7-on-the-6-years-ibm-x60-notebook/">re-install Windows 7 on IBM X60 based on the flash drive</a>, now the new bought Sandisk Ultra USB flash drive is no use now, so during the National Holiday, I just install a Live-CD type Linux on it so I can play something different with MS stuff latter.

I choose <a href="http://en.wikipedia.org/wiki/Knoppix" target="_blank">Knoppix</a> largely because of it's size: Knoppix has the largest size among other Linux distribution like puppy/Slax as well as it's popularity, you can <a href="http://www.knoppix.net/get.php" target="_blank">Get Knoppix</a> via a BT client like <a href="http://www.utorrent.com/" target="_blank">&micro;Torrent</a>. After you get the 3.68GB DVD ISO downloaded, the step to custom Knoppix to fit your needs will be quite simple (yes simple, but it must be consider in the Linux standard):

<!--more-->

<ol>
	<li>Write ISO Image to flash drive, using UltraISO (I use method USB-ZIP+) or <a href="http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/" target="_blank">Universl USB Installer</a>.</li>
	<li>Boot it and make a persistent store, I choose AES256 crypted and size 3627MB, you should be quite to input the number as at boot time, the screen has time counting.</li>
	<li>Setup the network by click the right corner of desktop applet: NetworkManager, it is quite easier to setup whether you use wirelss or wire network.</li>
	<li>Due to GFW in Chinese, you have to edit the /etc/hosts by some degree and add OpenDNS (208.67.222.222) if needed. You can running Chromium to verify the internet is OK now and download a copy of <a href="/media/agpzfmVyaWMtZ3Vvcg0LEgVNZWRpYRi5ggcM/hosts" target="_blank">host</a> to reference/replace your version.</li>
	<li>Install some additional components which can not deploy by Knoppix due to license limit: Start-&gt;Perferences-&gt;install components. (Flash and MS font, two package currently), Running Chromium again to <a href="http://www.adobe.com/software/flash/about/" target="_blank">verify the flash is install OK</a>, before that you have to change Preference in Chromium first via <a href="chrome://settings/content" target="_blank">chrome://settings/content</a>, to enable Plug-ins and JavaScript first.</li>
	<li>Install Chinese input method via Synapitc Package Manager (Start-&gt;Perference-&gt;Synapitc...) or via apt-get command in Accesories-&gt;Root Terminal (you may want to change background to semi trans as by default it is totally transpanrency via Edit-&gt;Perferences-&gt;Backgroud-&gt;Opacity at this moment):
<ol><strong>apt-get install scim-pinyin</strong></ol>
</li>
	<li>The above command will download ttf-arphic-ukai &amp; ttf-arphic-uming as well as scim-pinyin depend on it, to Switch to use scim input method now, you need running Start-&gt;Perference-&gt;Input Methods Switcher to "Use SCIM via IM_MODULE"</li>
</ol>
After above 7 steps, you can in fact to use Knoppix now, but you may also want to install additional software like <a href="http://www.rarlab.com/download.htm" target="_blank">WinRAR</a> (very popular but not free):&nbsp; you can first extract to home folder ~/, run make in ~/rar folder and put rarreg.key to /etc/ if you have a license.

I also need to use PyCharm 1.5.4 to do some Google App Engine programming, so still need manually install: (in Root Terminal running)
<ol><strong>apt-get install sun-java6-jre</strong></ol>
Because PyCharm say it must run Sun Java Runtime instead of Open Java Runtime, so it is need to install it first. You may also want to install some other program like <a href="http://psyco.sourceforge.net/introduction.html" target="_blank">python-psyco</a>, qtm (blogging client) or scribes (a new text editor) at this time with similar apt-get install command.

Download the <a href="http://code.google.com/appengine/downloads.html" target="_blank">GAE SDK</a>, current writing, I use <a href="http://googleappengine.googlecode.com/files/google_appengine_1.5.4.zip" target="_blank">version 1.5.4</a>, extract to home folder first, then move to /usr/local if you want in root, you may refer to that folder after install PyCharm, but it is another topic I think.

You may also have to edit your /etc/profile to export an environment to ensure PyCharm can find the correct java run time after line 24:
<ol><strong>PYCHARM_JDK=/usr/lib/jvm/java-6-sun/jre</strong></ol>
<ol><strong>export PYCHARM_JDK</strong></ol>
Now we can <a href="http://www.jetbrains.com/pycharm/download/" target="_blank">download the PyCharm 1.5.4</a> and extract &amp; move to /usr/local also. after that you can&nbsp; run /usr/local/pycharm-1.5.4/bin/pycharm.sh to test if every thing is OK, after run that script once, you can run PyCharm by run charm directly in Start-&gt;Run dialog.

I done to setup the Knoppix by above sequence and hoping you also go smooth with this blog while setting up your personal Knoppix USB flash drive.