---
layout: post
title: "How to install gem curb in Windows"
date: 2014-08-19T20:24:00+08:00
comments: true
external-url:
categories: [Ruby, Windows7]
---

After read the [post in Ruby-China say how to use mechanize and curl to login Ruby-China](https://ruby-china.org/topics/21120), I decide to install [curb](https://github.com/taf2/curb), seems still a little tricky.

First [download](http://curl.haxx.se/download.html) last available libcurl in windows, which currently is [7.34.0](http://curl.haxx.se/gknw.net/7.34.0/dist-w32/curl-7.34.0-devel-mingw32.zip).

Extract to C:\ and install the curb via below command.

```bat
gem install curb --platform=ruby -- --with-curl-lib=C:/curl-7.34.0-devel-mingw32/bin --with-curl-include=C:/curl-7.34.0-devel-mingw32/include
```

Some relative issues in github about curb, [#37](https://github.com/taf2/curb/issues/37), [#183](https://github.com/taf2/curb/issues/183)

To make [SSL certification work](http://curl.haxx.se/docs/sslcerts.html), you need to set your windows environment variable, the file can be download [here](http://curl.haxx.se/docs/caextract.html)

```
CURL_CA_BUNDLE=C:\Ruby200\bin\ca-bundle.crt
SSL_CERT_FILE=C:\Ruby200\bin\cacert.pem
```

Finally, need copy all DLL and EXE file in `C:\curl-7.34.0-devel-mingw32\bin` to `C:\Ruby200\bin`.