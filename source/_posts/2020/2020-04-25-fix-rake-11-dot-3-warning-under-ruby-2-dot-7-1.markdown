---
layout: post
title: "Fix rake 11.3 and jekyll 0.12.1 warning under ruby 2.7.1"
date: 2020-04-25T19:59:51+08:00
comments: true
external-url:
categories:
---

After upgrade ruby to 2.7.1, I found my old octopress give below two warning:

`/usr/local/lib/ruby/gems/2.7.0/gems/rake-11.3.0/lib/rake/application.rb:378: warning: deprecated Object#=~ is called on Proc; it always returns nil`

Just need change application.rb:378 as below to fix it.

```diff
-        opt.select { |o| o =~ /^-/ }.map(&:downcase).sort.reverse
+        opt.select { |o| o.is_a?(String) && o =~ /^-/ }.map(&:downcase).sort.reverse
```

Another warning is:

`/usr/local/lib/ruby/gems/2.7.0/gems/jekyll-0.12.1/lib/jekyll/post.rb:140: warning: URI.escape is obsolete`

Just change as below:

```diff
-          "categories" => categories.map { |c| URI.escape(c) }.join('/'),
+          "categories" => categories.map { |c| URI.encode_www_form_component(c) }.join('/'),
```
