---
layout: post
title: "Fix puma failed to restart in ruby 2.7.1"
date: 2020-04-06T22:30:47+08:00
comments: true
external-url:
categories: [Rails, Ruby]
---

After install ruby 2.7.1, found capistrano deploy via puma having below error when restart:

```text
/home/web_site/.rbenv/versions/2.7.1/bin/bundle:23:in `load': cannot load such file -- /home/web_site/.rbenv/versions/2.7.1/lib/ruby/gems/2.7.0/gems/bundler-2.1.4/libexec/bundle (LoadError)
```

After checking long time found it's just not install the bundler 1.17.3 as pumactl relay on it.


```bash
rbenv uninstall 2.6.5
rbenv global 2.7.1
rbenv shell 2.7.1
gem install bundler --default -v "1.17.3"
gem install bundler
```
