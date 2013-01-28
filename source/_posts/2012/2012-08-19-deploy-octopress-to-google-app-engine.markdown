---
layout: post
title: "Deploy/Hosting octopress to Google App Engine"
date: 2012-08-19 07:24
comments: true
external-url:
categories: [Cloud]
---
After I [switch](/2012/08/12/switch-from-micolog-to-octopress/) my blog to Octopress, I realize that compare with the Google App Engine, the [Github gh-pages hosting service](http://pages.github.com/) is much slower (10 times compare with GAE ping value, 6xxms vs 6xms)

So I believe it's better to hosting the static blog file to GAE, switch to GAE also enable server side script as a bonus if you didn't care about portable, which is the most reason I using Octopress.

<!--more-->

```yaml Just save as app.yaml at your octopress root folder
application: # replace your applyed GAE app id
version: 1
runtime: python27
api_version: 1
threadsafe: true

default_expiration: "15m"

handlers:
# index files
- url: /(.*)/
  static_files: public/\1/index.html
  upload: public/(.*)/index.html

# site root
- url: /
  static_files: public/index.html
  upload: public/index.html

- url: /robots.txt
  static_files: public/robots.txt
  upload: public/robots.txt

- url: /archives
  static_files: public/archives/index.html
  upload: public/archives/index.html

- url: /about
  static_files: public/about/index.html
  upload: public/about/index.html

- url: /resume
  static_files: public/resume/index.html
  upload: public/resume/index.html

- url: /categories
  static_files: public/categories/index.html
  upload: public/categories/index.html

# categories have no '/', so can not cover by index files rule
- url: /categories/(.*)
  static_files: public/categories/\1/index.html
  upload: public/categories/(.*)/index.html

- url: /
  static_dir: public
```

After creating GAE required app.yaml file, using `appcfg.py update .` to upload your Octopress blog to GAE, yes, certianly you also need [install GAE SDK first](https://developers.google.com/appengine/downloads) and apply a GAE account.

