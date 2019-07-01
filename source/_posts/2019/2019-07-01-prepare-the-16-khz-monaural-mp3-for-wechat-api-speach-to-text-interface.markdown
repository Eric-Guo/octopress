---
layout: post
title: "Prepare the 16 kHz Monaural MP3 for Wechat API speach to text interface"
date: 2019-07-01T10:33:07+08:00
comments: true
external-url:
categories:
---

```bash
ffmpeg -i test_voice.amr -acodec mp3 -ac 1 -ar 16000 test_voice.mp3
```

[微信AI开放接口](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=21516712282KzWVE), require that interface.

## How to install in CentOS 7 from source code

```bash
yum install nasm
cd /usr/local/src
wget http://downloads.sourceforge.net/lame/lame-3.100.tar.gz
tar -zxvf lame-3.100.tar.gz
cd lame-3.100
./configure --prefix=/usr/local
make && make install
ln -s /usr/local/lib/libmp3lame.so.0.0.0 /usr/lib64/libmp3lame.so.0
wget http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
tar -jxvf ffmpeg-snapshot.tar.bz2
cd ffmpeg
./configure --prefix=/usr/local --enable-libmp3lame
make && make install
ffmpeg
```

[Original post](https://www.jiweichengzhu.com/article/7f8f6b9ce88948f5b49d5eceecaf98b6)
