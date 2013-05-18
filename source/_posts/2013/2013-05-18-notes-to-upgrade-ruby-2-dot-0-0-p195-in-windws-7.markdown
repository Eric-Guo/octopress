---
layout: post
title: "Notes to upgrade ruby 2.0.0-p195 in Windws 7"
date: 2013-05-18T14:37:58+08:00
comments: true
external-url:
categories: [Ruby, Windows7]
---

Finally, I go to ruby 2.0, after the first patch p195 release, roughly three month [the ruby 2.0 released in its 20 years celebration](https://blog.heroku.com/archives/2013/3/6/matz_highlights_ruby_2_0_at_waza). I using the [rubyinstaller.org](http://rubyinstaller.org/downloads/) version and also it's devkit, the detail [installation procedure](/2013/01/28/howto-install-ruby-rails-rubymine-in-windows7/) is very like previous 1.9.3, so won't repeat here now, but I do found some trick which should in fact including in the official document but not, so I would list as below:

1. Need manually install the sqlite3.

    1. run C:\DevKit\devkitvars.bat
    2. mkdir c:\temp
    3. download http://packages.openknapsack.org/sqlite/sqlite-3.7.15.2-x86-windows.tar.lzma to c:\temp
    4. c:\Temp>bsdtar --lzma -xf sqlite-3.7.15.2-x86-windows.tar.lzma
    5. c:\Temp>gem install sqlite3 --platform=ruby -- --with-opt-dir=C:/Temp


2. Using below .gemrc and reinstall the gems like [yajl-ruby](https://rubygems.org/gems/yajl-ruby), [win32console](https://rubygems.org/gems/win32console) or [bcrypt-ruby](https://rubygems.org/gems/bcrypt-ruby) if you found the x86-mingw32 version can not work out of box.

```yaml edit/create the .gemrc as below content
---
:backtrace: false
:benchmark: false
:bulk_threshold: 1000
:sources:
- http://ruby.taobao.org
:update_sources: true
:verbose: true
gem: --no-document --platform=ruby
```

3. You may want to comment out the "DL is deprecated, please use Fiddle" warning at `C:\Ruby200\lib\ruby\2.0.0\dl.rb` since it's annoy and you are not the irb/pry or some other gems code owner...

[Ruby 2.0 performance](http://jp.rubyist.net/magazine/?Ruby200SpecialEn) is somewhat improved and it's worth to using it right now.