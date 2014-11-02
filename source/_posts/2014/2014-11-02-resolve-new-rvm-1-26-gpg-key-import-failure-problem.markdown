---
layout: post
title: "Resolve new RVM 1.26 GPG key import failure problem"
date: 2014-11-02T11:32:55+08:00
comments: true
external-url:
categories: [Ruby]
---

When the hkp port blocked, so rvm suggested `gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3` always failed to running, [here](http://superuser.com/questions/64922/how-to-work-around-blocked-outbound-hkp-port-for-apt-keys) is how to resolve such problem.

1. Find a hkp port not block server, run export after import the key:

	`gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3`
	`gpg --export --armor D39DC0E3`

2. Go to hkp port blocked server, run:

	`gpg --import -`

and copy and paste the step 1 server public key content and press `Ctrl+D`

done!