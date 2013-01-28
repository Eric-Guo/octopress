---
layout: post
title: "Install Ruby 1.9.3 p125 & Rails 3.2.6 on Windows 7"
date: 2012-04-01 17:27
comments: true
external-url:
categories: [Ruby, Rails, Windows7]
---
<a href="http://ruby-china.org/wiki/install_ruby_guide" target="_blank">Some blogs</a> says Windows is not a right platform to start Learning Ruby, maybe it's right, but anyway, I just successfully install the latest Ruby&amp;Rails web stack following below steps:

<!--more-->

<ol>
	<li>Install <a href="http://railsinstaller.org/" target="_blank">RailsInstaller</a> as a whole exe (56.6MB), if you want to access that file in China, please use <a href="http://q.115.com/100292/7984?p=all" target="_blank">this link</a> instead, you may have to <a title="my 115 invent code :-)" href="http://115.com/invite/731990" target="_blank">register 115 account</a> first.</li>
After install, For C:\RailsInstaller\scripts\config_check.rb, you need to change:

<blockquote>:home&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; =&gt; File.join( ENV["HOMEDRIVE"], ENV["HOMEPATH"] ),
:ssh_path&nbsp;&nbsp;&nbsp; =&gt; File.join( ENV["HOMEDRIVE"], ENV["HOMEPATH"], ".ssh" ),
:ssh_key&nbsp;&nbsp;&nbsp;&nbsp; =&gt; File.join( ENV["HOMEDRIVE"], ENV["HOMEPATH"], ".ssh", "id_rsa"),
:ssh_pub_key =&gt; File.join( ENV["HOMEDRIVE"], ENV["HOMEPATH"], ".ssh", "id_rsa.pub"),</blockquote>

Into:
<blockquote>:home&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; =&gt; ENV["HOME"],
:ssh_path&nbsp;&nbsp;&nbsp; =&gt; File.join( ENV["HOME"], ".ssh" ),
:ssh_key&nbsp;&nbsp;&nbsp;&nbsp; =&gt; File.join( ENV["HOME"], ".ssh", "id_rsa"),
:ssh_pub_key =&gt; File.join( ENV["HOME"], ".ssh", "id_rsa.pub"),</blockquote>

And create %HOME% environment in System Properties-&gt;Advanced-&gt;Environment Variables and REM the SET HOME=%HOMEDRIVE%%HOMEPATH% in c:\RailsInstaller\Ruby1.9.3\setup_environment.bat, then you should be able to run "Command Prompt with Ruby and Rails"
	<li>gem source -r http://rubygems.org/ # if you have VPN, you can skip step 2 &amp; 3</li>
	<li>gem source -a http://ruby.taobao.org/</li>
	<li>gem update &ndash;&ndash;system</li>
	<li>gem update</li>
	<li>gem install win32console #enable colorize_logging in windows</li>
	<li>gem install vmc #if plan to deploy your rails application in Cloud Foundry</li>
	<li>gem install ruby-oci8 #if plan to deploy your rails application in Oracle DB)</li>
	<li>gem install activerecord-oracle_enhanced-adapter #if plan to deploy your rails application in Oracle DB</li>
	<li>gem install pry #an IRB alternative and runtime developer console</li>
	<li>gem install pry-doc</li>
	<li>gem install mongoid</li>
	<li>gem install bson_ext</li>
	<li>gem install <a href="http://stackoverflow.com/questions/9705947/installing-ruby-debug-base19-on-windows-in-ruby-1-9-3" target="_blank">linecache19 </a>#below three line is need if you want to debug with RubyMine</li>
	<li>gem install ruby-debug-ide</li>
	<li>gem install <a href="http://rubyforge.org/frs/?group_id=8883" target="_blank">ruby-debug-base19-0.11.26.gem</a> -- --with-ruby-include="C:\RailsInstaller\Ruby1.9.3\include\ruby-1.9.1\ruby-1.9.3-p125"</li>
	<li>gem install specific_install</li>
	<li>gem specific_install -l http://github.com/eventmachine/eventmachine.git # build eventmachine or use below to install a precompile version</li>
	<li>gem install eventmachine --pre # version eventmachine-1.0.0.rc.4-x86-mingw32.gem works in Windows 7</li>
	<li>gem install thin # new version 1.4.1 need add gem 'thin' in Gemfile to work successfully</li>
	<li>gem clean (clean some outdated gems during above installation, say n if any depend warning)</li>
</ol>
After install and if you already install Git in "C:\Program Files\Git" like me, you can delete the Git folder "C:\RailsInstaller\Git" installed by RailsInstaller and make a Junction link to your existing one (need in the administrator cmd prompt) to save 250+ MB hard drive.
<blockquote>C:\windows\system32&gt;mklink /J C:\RailsInstaller\Git "C:\Program Files\Git"
Junction created for C:\RailsInstaller\Git &lt;&lt;===&gt;&gt; C:\Program Files\Git</blockquote>
You can also edit the file "C:\RailsInstaller\Ruby1.9.3\lib\ruby\1.9.1\webrick\httpresponse.rb", line 205 and add red part to <a href="http://stackoverflow.com/questions/7082364/what-does-warn-could-not-determine-content-length-of-response-body-mean-and-h" target="_blank">avoid very annoying warning</a>.

```ruby
if chunked? || @header['content-length'] <span style="color: #ff0000;">|| @status == 304 || @status == 204</span>
```