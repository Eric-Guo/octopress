---
layout: post
title: "Using POW and Byebug together"
date: 2017-04-20T21:21:42+08:00
comments: true
external-url: 
categories: [Rails]
---

Some rails application, especially in SaSS category will behave differently based on the different subdomain. in that case, [pow](http://pow.cx) is necessary, as it makes simulate subdomain much easier compare with edit based on `/etc/hosts`.

Setup the pow relative easy:

```bash
brew install pow
mkdir -p ~/Library/Application\ Support/Pow/Hosts
ln -s ~/Library/Application\ Support/Pow/Hosts ~/.pow
pow --install-local
launchctl load -w ~/Library/LaunchAgents/cx.pow.powd.plist
```

Then you can link required rails application via:

```bash
cd ~/.pow
ln -s /Users/guochunzhong/git/product_hunt/ product-hunt
```

And finally, using [http://product-hunt.dev](http://product-hunt.dev) to access rails application without using `rails s`.

Because now there is no terminal to running `rails s`, so you now have to use `tail -f log/development.log` to monitor, it’s not a big deal, in fact, more flexible as it enables you Ctrl+C any time and leaves the interesting log in the terminal to read.

The big issue is now you are no access the [Byebug](https://github.com/deivid-rodriguez/byebug) as even Byebug is paused, we have no access to it, so must using remote mode of byebug.

Luckily, it’s still not too hard, just following [product_hunt commit](https://github.com/Eric-Guo/product_hunt/commit/b995c82103d324f480d3c76a8a6943feee6f9c23) will do the job.

