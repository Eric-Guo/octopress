---
layout: post
title: "Deploy new rails 6 application in Ubuntu 20.04"
date: 2021-04-29T13:31:39+08:00
comments: true
external-url: 
categories: [Rails Ubuntu]
---

# Create new user

```bash
adduser cybros_vendor
sudo su - cybros_vendor
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys # and paste your public key
chmod 600 .ssh/authorized_keys
```

# Install node.js 15 and yarn 1.x

Using [nodesource distribution](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)

```bash
curl -fsSL https://deb.nodesource.com/setup_15.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install gcc g++ make
## To install the Yarn package manager
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```
