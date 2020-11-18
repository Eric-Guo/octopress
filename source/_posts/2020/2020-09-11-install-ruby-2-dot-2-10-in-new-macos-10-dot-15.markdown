---
layout: post
title: "Install ruby 2.2.10 in new macOS 10.15 and Linux"
date: 2020-09-11T11:04:26+08:00
comments: true
external-url:
categories: [Ruby, macOS]
---

```bash
RUBY_CONFIGURE_OPTS=--disable-werror rbenv install 2.2.10
```

See [doc](https://github.com/rbenv/ruby-build#advanced-usage) if require.

If in Linux:

```bash
sudo apt install gcc-6 g++-6
wget https://github.com/openssl/openssl/archive/OpenSSL_1_0_2j.tar.gz
tar xzf OpenSSL_1_0_2j.tar.gz
cd openssl-OpenSSL_1_0_2j
./config --prefix=/opt/openssl/1.0.2j
make depend
make -j$(nproc)
sudo make install
CC=$(which gcc-6) CONFIGURE_OPTS="--with-openssl-dir=/opt/openssl/1.0.2j" rbenv install 2.2.10
```
