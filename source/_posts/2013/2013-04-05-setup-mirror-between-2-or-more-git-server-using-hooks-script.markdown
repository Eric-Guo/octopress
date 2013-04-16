---
layout: post
title: "Setup mirror between 2 or more git server using hooks script"
date: 2013-04-05T21:44:47+08:00
comments: true
external-url:
categories: [Git]
---

First make sure the two server can using ssh to login each other, then add remote mirror to bare git repository.

```bash
git remote add --mirror=push cvpgitip01 git@10.11.37.110:repositories/Feng-Wu/demo.git
```

Add hooks `post-commit` in bare git repository.

```bash
#!/bin/sh
exec git push cvpgitip01 -f --mirror
```