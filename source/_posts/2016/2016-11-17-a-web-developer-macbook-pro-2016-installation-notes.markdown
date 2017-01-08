---
layout: post
title: "A rubyist web developer Macbook Pro with touch bar installation notes"
date: 2016-11-17T20:17:49+08:00
comments: true
external-url: 
categories: [macOS, MBP]
---

Just got the Macbook Pro 2016 with touch bar 15' edition, setting it up takes my 1 full day. If the China network condition is good, I believe it will takes much less time.

I decide the Macbook Pro from scratch instead of using migration assist because I want to keep the system clean. I will install as much application as possible from App Store, then the [Homebrew](http://brew.sh), if both software repositry can not find it, install it manually.

Apple store installation is the easiest one, once you login and you will got all the application you have purchased.

brew are also relative easier, I even using brew to install Chrome and Firefox, via `brew cask install google-chrome`.

I would like to using percona instead of mysql because in China, Aliyuj RDS is largely based on percona, so I perfer using same software in dev env.

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
vi /usr/local/Cellar/proxychains-ng/4.11/etc/proxychains.conf
```

It will finally takes 76Gb after install all my needs, about 33% used, so I think buy 256Gb SSD edition of rMBP should be enought for my next 4 years usage.