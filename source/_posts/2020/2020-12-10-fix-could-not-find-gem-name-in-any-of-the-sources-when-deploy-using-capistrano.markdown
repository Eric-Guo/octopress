---
layout: post
title: "Fix Could not find gem_name in any of the sources when deploy using capistrano"
date: 2020-12-10T10:50:49+08:00
comments: true
external-url: 
categories: [capistrano, bundler]
---

When do running capistrano `bundler:install` stage, I always meet below error:

```text
00:32 bundler:install
      01 $HOME/.rbenv/bin/rbenv exec bundle install --jobs 4 --quiet
      01 Could not find canonical-rails-0.2.9 in any of the sources
#<Thread:0x00007f92c4af3cd8 /usr/local/lib/ruby/gems/2.7.0/gems/sshkit-1.21.1/lib/sshkit/runners/parallel.rb:10 run> terminated with exception (report_on_exception is true):
Traceback (most recent call last):
  13: from /usr/local/lib/ruby/gems/2.7.0/gems/sshkit-1.21.1/lib/sshkit/runners/parallel.rb:12:in `block (2 levels) in execute'
  12: from /usr/local/lib/ruby/gems/2.7.0/gems/sshkit-1.21.1/lib/sshkit/backends/abstract.rb:31:in `run'
  11: from /usr/local/lib/ruby/gems/2.7.0/gems/sshkit-1.21.1/lib/sshkit/backends/abstract.rb:31:in `instance_exec'
  10: from /usr/local/lib/ruby/gems/2.7.0/gems/capistrano-bundler-2.0.1/lib/capistrano/tasks/bundler.cap:57:in `block (3 levels) in <top (required)>'
```

After several try debug, found can remove below folder to resolve the problem.

```bash
rm -rf /var/www/thape_web/shared/bundle/ruby/2.7.0/cache/bundler/git/canonical-rails-866574f7436cf4f082de6fa4c0958c8dccaccc7a
rm -rf /var/www/thape_web/shared/bundle/ruby/2.7.0/bundler/gems/canonical-rails-*
```

Maybe do `gem update --system` and `gem install bundler && bundler config default 2.1.4` also help, but at least not the root cause of such error fix.
