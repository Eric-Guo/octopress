---
layout: post
title: "How to avoid the ruby debugger warning already initialized constant VERSION in Rubymine"
date: 2013-01-04 16:10
comments: true
external-url:
categories: [Ruby, Rails]
---
Now I'm using the [Rubymine](http://www.jetbrains.com/ruby/) instead of pry-debugger as my primary Rails environment, Rubymine has much great debug facility and what's more, I spend $17 when the [JetBrains offer 75% off at the Mayan Doomsday](http://www.quora.com/JetBrains/How-many-units-did-JetBrains-sell-at-the-Mayan-Doomsday-sale).

But I found a problem that rubymine always refuse to enter the debug mode when I using my [pl-form](https://github.com/Eric-Guo/pl-form) project in below warning:

```
debugger-1.2.3/lib/ruby_debug.so: warning: already initialized constant VERSION
```

finally I found prject Gemfile need a little review:
```ruby
group :development do
  gem 'quiet_assets'
  # Disable below two line if you using rubymine
  #gem 'pry-rails'
  #gem 'pry-debugger'
end
```
