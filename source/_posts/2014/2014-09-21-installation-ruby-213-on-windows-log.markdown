---
layout: post
title: "Installation Ruby 2.1.3 on Windows log"
date: 2014-09-21T09:27:04+08:00
comments: true
external-url:
categories: [Ruby, Windows7]
---

Except simply install the ruby 2.1.3 from [rubyinstaller](http://rubyinstaller.org/downloads/) and it's DevKit, here is my log when I meet the problem during ruby 2.1.3 on a Windows 7 32bits machine.

1. Comments out the **warn "DL is deprecated, please use Fiddle"** at `C:\Ruby21\lib\ruby\2.1.0\dl.rb`
1. Install yajl-ruby via `gem install yajl-ruby -v 1.1.0 --platform ruby`
1. Install RedCloth via `gem install RedCloth --platform ruby` and move the file `C:\Ruby21\lib\ruby\gems\2.1.0\gems\RedCloth-4.2.9\lib\redcloth_scan.so` to new created folder `C:\Ruby21\lib\ruby\gems\2.1.0\gems\RedCloth-4.2.9\lib\2.1`.
1. Install sqlite3 via:

    1. run `C:\DevKit\devkitvars.bat`
    2. `mkdir c:\temp`
    3. download http://packages.openknapsack.org/sqlite/sqlite-3.7.15.2-x86-windows.tar.lzma to c:\temp
    4. c:\Temp>`bsdtar --lzma -xf sqlite-3.7.15.2-x86-windows.tar.lzma`
    5. c:\Temp>`gem install sqlite3 --platform=ruby -- --with-opt-dir=C:/Temp`
1. Install bcrypt via `gem install bcrypt --platform ruby`
1. Install win32console via `gem install win32console --platform ruby`
1. Install ffi via `gem install ffi --platform ruby`
1. Install pg via `gem install pg --platform ruby`
1. Install mysql following [stackoverflow](http://stackoverflow.com/questions/19014117/ruby-mysql2-gem-installation-on-windows-7) via `gem install mysql2 --platform=ruby -- '--with-mysql-dir="C:\mysql-connector"'`
1. [Install](https://github.com/hicknhack-software/rails-disco/wiki/Installing-puma-on-windows) puma via `gem install puma -- --with-opt-dir=c:\temp`

Setting global environment setting:

```ini
CURL_CA_BUNDLE=C:\Ruby21\share\ca-bundle.crt
SSL_CERT_FILE=C:\Ruby21\share\cacert.pem
NLS_LANG=AMERICAN_AMERICA.UTF8
```

Also do not using ansicon in ruby 2.1.3 any more, seems not compatible.