---
layout: post
title: "Reinstall Macbook 2016 with MacOS 10.14.4"
date: 2019-02-25T15:40:07+08:00
comments: true
external-url:
categories:
---

Two years after running [MacOS 10.12.6](/2016/11/17/a-web-developer-macbook-pro-2016-installation-notes/), I decide to switch to a new job so have to re-install my MBP to 10.14.4 to make sure nothing left in my old computer.

Here is the list of application/software/tools currently heavy use:

MacStore apps:

```bash
Evernote
Xcode
QQ
Wechat
Aware
Pages
Numbers
Keynote
iMovie
Elmedia Video Player
Pixelmator
PDF Export
Squash
Affinity Designer
Youku
Drop - Color Picker
Polarr Photo Editor Pro
Mate
腾讯视频
Telegram Desktop
Tweetbot 2
AdGuard for Safari
```

Install tools via brew

```bash
brew install ansible
brew install bash
brew install elasticsearch
brew install eslint
brew install go
brew install hub
brew install jenv
brew install jq
brew install memcached
brew install mtr
brew install node
brew install overmind
brew install p7zip
brew install pandoc
brew install percona-server
brew install postgresql
brew install prettier
brew install proxychains-ng
brew install puma-dev
brew install redis
brew install ruby
brew install sqlite
brew install sshuttle
brew install unrar
brew install vim
brew install yamllint
brew install yarn
```

Install tools via brew cask

```
brew cask install adoptopenjdk8
brew cask install airserver
brew cask install anaconda
brew cask install chromedriver
brew cask install data-integration
brew cask install firefox
brew cask install google-chrome
brew cask install googleappengine
brew cask install java
brew cask install paw
brew cask install rubymine
brew cask install sourcetree
brew cask install sublime-text-dev
brew cask install surge
brew cask install typora
brew cask install viscosity
brew cask install zoomus
```

Install from web / download:

```
Excel
PowerPoint
Word
Remote Desktop Connection
SQLPro Studio
微信开发者工具
百度网盘
```

Append below lines to `.bash_profile` to activate conda.

```bash
source /usr/local/anaconda3/etc/profile.d/conda.sh
conda activate
```

New brew relay on CommandLineTools, acturally there is no need and node need xcode existing, so run below to fix.

```bash
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/
```

Some gem need special handle

```bash
gem install libxml-ruby -v '3.1.0' -- --use-system-libraries=true --with-xml2-include="$(xcrun --show-sdk-path)"/usr/include/libxml2
gem install nokogiri -v '1.10.2' -- --use-system-libraries=true --with-xml2-include="$(xcrun --show-sdk-path)"/usr/include/libxml2
```
