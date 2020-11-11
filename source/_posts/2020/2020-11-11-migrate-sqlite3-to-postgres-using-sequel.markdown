---
layout: post
title: "Migrate sqlite3 to postgres using Sequel"
date: 2020-11-11T11:17:34+08:00
comments: true
categories: [Ruby, Sequel, 'DB Migrate']
---

I used to using [mysql-postgresql-converter](https://github.com/mounty1/mysql-postgresql-converter), but due to my new site is using sqlite3, so that tips not working.

But I found a much *general* DB tools at [SO](https://stackoverflow.com/a/34726143/262826)

```bash
gem install sequel
sequel -C sqlite:'/Users/user_name/git/sso/thape_web/db/development.sqlite3' postgres://localhost/thape_web_dev
```
