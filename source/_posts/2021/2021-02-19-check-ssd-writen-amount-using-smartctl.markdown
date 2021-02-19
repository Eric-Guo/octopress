---
layout: post
title: "Check SSD writen amount using smartctl"
date: 2021-02-19T11:43:42+08:00
comments: true
external-url: 
categories: [MacOS]
---

```bash
brew install smartmontools
sudo smartctl -a /dev/disk0
# Percentage Used:                    6%
# Data Units Read:                    67,399,384 [34.5 TB]
# Data Units Written:                 53,574,524 [27.4 TB]
```

So my 4 years MBP 2016 used 6% of the SSD life time.
