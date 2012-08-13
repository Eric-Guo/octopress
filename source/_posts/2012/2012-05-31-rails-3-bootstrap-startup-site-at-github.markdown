---
layout: post
title: "Rails 3.2 Bootstrap startup site at Github"
date: 2012-05-31 17:47
comments: true
external-url:
categories: [Rails, Ruby, HTML5]
---
60 days passed since I decide to <a href="/2012/03/31/some-thought-after-attending-the-cloud-foundry-2012-tour-in-shanghai">learn Ruby &amp; Rails after I attended the Cloud Foundry 2012 tour</a>. I'm now somewhat familiar with Rails 3.2, I following the <a href="http://huacnlee.com/blog/how-to-start-learning-ruby-on-rails" target="_blank">learning roadmap proposed by huacnlee</a> and found most useful resource for Rails is still the <a href="http://guides.rubyonrails.org/" target="_blank">official Ruby on Rails Guide</a>. So if you also want to learn the Rails, I recommend you only read that.

I also start/participate 3 rails project during the passed 60 days<!--more-->, first one is called Spare Part Management System, my first real rails application which only internally use in my company, the second one is <a href="https://github.com/kenshin54/ScriptCode" target="_blank">ScriptCode</a>, an <a href="http://topgeek.org/?p=399" target="_blank">Hackathon</a> project written in 24 hours! I must give big thanks to my temp team mate, I really learn quite a lot of in that 24 hours. And currently I'm working on project, Paper-Less Forms production log system, which also internal use.

So as a result of create and customize over and over again via <em>rails new myapp</em> command, I decide to start a new startup site, which can be more focus on enterprise application, so this is why I start the <a href="https://github.com/Eric-Guo/bootstrap-rails-startup-site" target="_blank"><strong>Bootstrap-Rails startup site</strong></a> open source project. since in enterprise, the frontend UI is seldom change, so using the Bootstrap is quite good choose, I choose the <a href="http://nex-3.com/posts/100-convert-less-to-scss" target="_blank">Sass instead of less</a> as CSS language just because Sass is pure ruby and work great in windows, the less Gems need therubyracer, which not available in windows.

<h5>The job which already do for you</h5>
<ol>
	<li>using Sass based Bootstrap template</li>
	<li>using ruby.taobao.org in Gemfile</li>
	<li>add win32console for more beautiful out in Windows</li>
	<li>config.assets.debug = false in config/environments/development.rb to focus function at first</li>
	<li>add developer document reference</li>
	<li>default Sublime Text 2 project</li>
	<li>Devise based authentication system</li>
	<li>simple_form style view for Devise</li>
</ol>

<h5>The job after you clone this site (how to start)</h5>
<ol>
	<li>bundle install</li>
	<li>rake db:migrate</li>
	<li>rake assets:precompile (this will give you very clean log out)</li>
	<li>rails server</li>
	<li>writing code or what ever you want</li>
</ol>

Hoping you would also like ruby and rails as me when you start writing something unique on the base of <strong><a href="https://github.com/Eric-Guo/bootstrap-rails-startup-site">bootstrap-rails-startup-site</a></strong>