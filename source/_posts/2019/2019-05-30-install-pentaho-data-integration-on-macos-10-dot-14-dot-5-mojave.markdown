---
layout: post
title: "Install Pentaho Data Integration on MacOS 10.14.5 Mojave"
date: 2019-05-30T15:47:53+08:00
comments: true
external-url:
categories:
---

It's a little hard to find out the root cause why out of box Pentaho Data Integration not working, any way, it's resolved.

First need java 8 to install, because Oracle refuse to provide, need OpenJDK instead.

```bash
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk8
brew install jenv # If also want to using other java version
cd "/Applications/Data Integration.app/Contents/MacOS"
cat JavaApplicationStub
```
Also need change `JavaApplicationStub` file as below. (Maybe Pentaho developer didin't having a MBP...)

```sh
#!/bin/sh

# PROG_DIR=$(cd "$(dirname "$0")"; pwd)

# PROG_DIR is in .app/Contents/MacOS

# BASE_DIR="$PROG_DIR"/../../../
BASE_DIR="/usr/local/Caskroom/data-integration/8.2.0.0-342/data-integration"
cd "$BASE_DIR"
echo $BASE_DIR
. "spoon.command" "$BASE_DIR"
```

If you want to continue using Java 12 in system wide, install `brew install jenv` and running `jenv local 1.8` at data-integration folder.
