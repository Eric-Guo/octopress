---
layout: post
title: "Change devise user password in rails console"
date: 2013-01-23T10:39:12+08:00
comments: true
external-url:
categories: [Rails]
---
```ruby
# $ rails console production
u=User.where(:email => 'usermail@gmail.com').first
u.password='userpassword'
u.password_confirmation='userpassword'
u.save
```