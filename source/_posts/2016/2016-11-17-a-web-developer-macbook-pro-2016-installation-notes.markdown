---
layout: post
title: "A rubyist web developer Macbook Pro with touch bar installation notes"
date: 2016-11-17T20:17:49+08:00
comments: true
external-url: 
categories: [macOS, MBP]
---

Just got the Macbook Pro 2016 with touch bar 15' edition, setting it up takes my one full day. If the China network condition is good, I believe it will take much less time.

I decide the Macbook Pro from scratch instead of using migration assist because I want to keep the system clean. I will install as much application as possible from App Store, then the [Homebrew](http://brew.sh), if both software repository can not find it, install it manually.

Apple store installation is the easiest one, once you log in and you will get all the application you have purchased.

brew are also relative easier, I even using brew to install Chrome and Firefox, via `brew cask install google-chrome`.

I would like to use percona instead of MySQL because, in China, Aliyun RDS is primarily based on percona, so I favor using the same software in dev env.

But you can not simply install percona directly.

## Install mysql, then percona to avoid pitfall

```bash
brew install mysql
brew services start mysql
brew services stop mysql
brew remove mysql
brew install percona-server
brew services start percona-server
```

## twitter cli need to rename tt instead of t

```bash
gem install t
mv /usr/local/bin/t /usr/local/bin/tt
```

## Nokogiri using system library

```bash
brew install libxml2
# If installing directly
gem install nokogiri -- --use-system-libraries \
  --with-xml2-include=$(brew --prefix libxml2)/include/libxml2
# If using Bundle
bundle config build.nokogiri --use-system-libraries \
  --with-xml2-include=$(brew --prefix libxml2)/include/libxml2
bundle install
```

## proxychains using Shadowsocks

To add Shadowsocks setting for proxychains.

```bash
# add below at end
# socks5  127.0.0.1 1080
vi /usr/local/etc/proxychains.conf 
```

It will finally take 76Gb after installing all my needs, about 33% used, so I believe in buying a 256Gb SSD edition of rMBP should be enough for my next four years usage.
