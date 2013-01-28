---
layout: post
title: "Building a centrol Linux GIT server based on gitolite in the Windows 7 using PuTTY as Client"
date: 2012-03-10 17:18
comments: true
external-url:
categories: [Git, Windows7, SSL]
---
I'm using a Windows 7 as my working notebook and usually I use PuTTY as SSH client to linux machine. Recently my Boss plan to implement a dedicated SCM server using GIT and after do some internet research, I found some <a href="http://www.geekgumbo.com/2011/10/18/ssh-and-the-gitolite-installation-part-2/" target="_blank">blogs</a> saying do not using putty and gitolite together. Certainly, it is not true and here is how to build a dedicated centrol.

<!--more-->

I assure you have already install the <a href="http://www.chiark.greenend.org.uk/%7Esgtatham/putty/download.html" target="_blank">PuTTY</a> (v0.62) and <a href="http://code.google.com/p/gitextensions/downloads/list" target="_blank">GIT Extension</a> (Complete edition already include KDiff3, msysGIT, no need install separately), you also already have a running linux server (using Ubuntu 10.04.4) and setting up the SSH and network ready and be able to login via PuTTY.

##### Step 1, prepare the user gitolite RSA key in the putty (in Windows 7 machine)

Running PuTTY Key Generator and generate a SSH-2 RSA 2048 bit key, the PuTTY key file name like cvpmesgit1-admin.ppk, password protection is recommended.

##### Step 2, enable SSH login via PuTTY using the key you just generated: (now switch to the Linux server)

generate the public key file id_rsa_gitolite.pub by copy those string from PuTTY Key Generator.

make sure using the id_rsa_gitolite.pub as default user .ssh/authorized_keys as well and can successfully login via ssh.

##### Step 3, create user gitolite in default user or root:

```bash
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'git version control' \
  --group \
  --disabled-password \
  --home /home/gitolite gitolite
```

##### Step 4, copy the public key to the gitolite account:

```bash
sudo cp id_rsa_gitolite.pub /home/gitolite
sudo chown gitolite:gitolite /home/gitolite/id_rsa_gitolite.pub
```

##### Step 5, Become the gitolite user:

```bash
sudo su - gitolite
```

##### Step 6, Clone the lastest gitolite from gitHub.com (if you are behind the firewall, replace git to http):

```bash
git clone git://github.com/sitaramc/gitolite
```

##### Step 7, Install the gitolite:

```bash
cd gitolite
src/gl-system-install
cd ~
```

##### Step 8, Setting the gitolite path

```bash
echo "PATH=/home/gitolite/bin:$PATH" &gt;&gt; ~/.bash_profile
source ~/.bash_profile
```

##### Step 9, setup the first user, just quite vim to configure file

```bash
gl-setup id_rsa_gitolite.pub
```

##### Step 10, clone the gitolite@cvpmesgit1:gitolite-admin in the GIT Extension (we are back to Windows 7) and you can admin it using this repository using PuTTY in the Windows 7 environment now!

You can also continue to build gitweb and git-daemon following <a href="http://computercamp.cdwilson.us/git-gitolite-git-daemon-gitweb-setup-on-ubunt" target="_blank">this blog</a>, the rest after building gitolite is almost all right.