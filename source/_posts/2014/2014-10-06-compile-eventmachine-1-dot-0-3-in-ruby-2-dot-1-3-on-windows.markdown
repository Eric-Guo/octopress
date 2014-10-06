---
layout: post
title: "Compile eventmachine 1.0.3 in Ruby 2.1.3 on Windows"
date: 2014-10-06T21:01:55+08:00
comments: true
external-url:
categories: [Ruby, Windows7]
---

[eventmachine](https://rubygems.org/gems/eventmachine) didn't release a new gems for a long time, so we have to manually compile if you decide using [Ruby 2.1.3](/2014/09/21/installation-ruby-213-on-windows-log/), it's probably the most complex gems which can still *successfully install* on windows as far as I known, here is the steps for your interesting:

	git clone https://github.com/eventmachine/eventmachine
	cd eventmachine && bundle install

Now you will meet the [bluecloth](https://rubygems.org/gems/bluecloth) can not install problem first, which you can following [stackoverflow](http://stackoverflow.com/questions/4932221/bluecloth-v2-0-10-with-windows-7-not-working) steps including patch [ext/bluecloth.h](https://gist.github.com/1539611) and 6 steps.

Now you need using updated version of [rake-compiler](https://rubygems.org/gems/rake-compiler) (v0.9.3) to continue, so change eventmachine.gemspec to that version and do the `bundle install` again.

Now we can running `rake package` and it should generate the *updated* 1.0.3 gem file you need, but before that you need another depend lzma package mentioned in [Ruby Installer Google Group](https://groups.google.com/forum/#!topic/rubyinstaller/e1DdfLy6-J0), I only tested [x86](http://packages.openknapsack.org/openssl/openssl-1.0.0m-x86-windows.tar.lzma) but the [x64](http://packages.openknapsack.org/openssl/openssl-1.0.0m-x64-windows.tar.lzma) should be same.

1. Put the download openssl-1.0.0m-x86-windows.tar.lzma to `c:\temp`
2. run `C:\DevKit\devkitvars.bat`
3. c:\Temp> `bsdtar --lzma -xf sqlite-3.7.15.2-x86-windows.tar.lzma`
4. c:\git\eventmachine\pkg> `gem install ./eventmachine-1.0.3.gem  --platform=ruby -- --with-opt-dir=C:/Temp`

Now enjoy eventmachine on ruby 2.1.3 windows!