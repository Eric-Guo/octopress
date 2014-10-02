---
layout: post
title: "Howto install Ruby, Rails, RubyMine in Windows 7"
date: 2013-01-28T15:20:53+08:00
comments: true
external-url:
categories: [Ruby, Rails, Windows7]
---

10 months ago, I start my Ruby & Rails life, [installation method at that time](/2012/04/01/install-ruby-1-dot-9-3-and-rails-3-dot-2-3-on-windows-7/) is greatly improve after that time, the Rails Installer also seems stop maintain since then. So here is another more professional way to install Ruby & Rails in Windows:

<!--more-->

1. Download [RubyInstaller and DevKit](http://rubyinstaller.org/downloads/) instead of Rails Installer, which is more frequently update.

2. Install RubyInstaller at `C:\Ruby193` (must Driver C, other will cause some sophisticated problem in windows).

3. Extract DevKit.exe to `C:\DevKit`.

4. Binding the DevKit to Ruby, [in order to install C extension gems later](http://rubyinstaller.org/add-ons/devkit/):

	```bat
	C:\git>cd \devkit
	C:\DevKit>ruby dk.rb init
	C:\DevKit>ruby dk.rb install --force
	```

5. Install [ansicon](https://github.com/adoxa/ansicon) - Process ANSI escape sequences for Windows console programs, extract the ansicon.exe to `C:\Windows`

6. Change `Programs\Ruby 1.9.3-p374\Start Command Prompt with Ruby` shortcut properties, append `C:\Windows\ansicon.exe ` at the begining to Shortcut->Target.

7. Install [GitExtensions](http://code.google.com/p/gitextensions/downloads/list), version 2.44 Setup Complete recommand but other version may also very stable, if you download not Complete version, also need to install [msysgit](http://code.google.com/p/msysgit/downloads/list) and [kdiff3](http://kdiff3.sourceforge.net/).

8. Install [node.js](http://nodejs.org/), latest version, which need by some gems like [execjs](https://github.com/sstephenson/execjs#readme).

9. Install [ActivePython](http://www.activestate.com/activepython/downloads), version 2.7, which need by some gems like [pygments](https://github.com/tmm1/pygments.rb).

10. Enter `Start Command Prompt with Ruby`, make sure git is available by `git version`.

11. `git clone git://github.com/Eric-Guo/bootstrap-rails-startup-site.git` to clone a rails template application.

12. Install RubyGems by below command

	```bat
	C:\git>gem update --system
	C:\git>gem upgrade
	C:\git>gem install pry-debugger
	C:\git>gem install bundle
	C:\git>gem install ruby-oci8
	C:\git>cd pl-form
	C:\git\pl-form>bundle install
	```

13. Install [RubyMine](http://www.jetbrains.com/ruby/download/index.html), open `C:\git\pl-form` as Directory and then click Debug(Shift-F9) to install relative debug gems.

14. Patch webbrick [httpresponse.rb file](file://C:/Ruby193/lib/ruby/1.9.1/webrick/httpresponse.rb) line 204:

    ```ruby
        #if chunked? || @header['content-length']
        if chunked? || @header['content-length'] || @status == 304 || @status == 204
    ```

#### Congratulation, now you are now professional rubyist in Windows now!