---
layout: post
title: "Presenting code with syntax-highlight in Keynote using command tool highlight"
date: 2020-07-09T08:24:28+08:00
comments: true
external-url:
categories:
---

# Just one line

```bash
pbpaste | highlight --syntax=rb -O rtf | pbcopy
pbpaste | highlight --syntax=html -O rtf | pbcopy
pbpaste | highlight --syntax=scss -O rtf | pbcopy
pbpaste | highlight --syntax=jsx -O rtf | pbcopy
pbpaste | python -m json.tool | highlight --syntax=json -O rtf | pbcopy
```

[Click](http://www.andre-simon.de/doku/highlight/en/langs.php) to see more language highlight supported.

Note: install highlight first using `brew install highlight`, [original link](https://gist.github.com/jimbojsb/1630790)
