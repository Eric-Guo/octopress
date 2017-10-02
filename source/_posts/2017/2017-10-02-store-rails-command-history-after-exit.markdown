---
layout: post
title: "Store rails command history after exit"
date: 2017-10-02T11:50:21+08:00
comments: true
external-url: 
categories: [Ruby, Rails]
---

Create or edit your ~/.irbrc file to include:

```bash
require 'irb/ext/save-history'
IRB.conf[:SAVE_HISTORY] = 2000
IRB.conf[:HISTORY_FILE] = "#{ENV['HOME']}/.irb-history"
```

[Original link](https://makandracards.com/makandra/24745-persist-rails-or-irb-console-command-history-after-exit)
