---
layout: post
title: "Switch from Micolog to Octopress"
date: 2012-08-12 18:30
comments: true
external-url:
categories: [Life, Cloud]
---
Just very glad to annource that I just finishing switch my blog from [Micolog](http://blog.xuming.net/2012/06/micolog_rebuild.html) to [Octopress](http://octopress.org/).

Micolog is a Google App Engine based blog, when I decide to use at [Auguest 2011](/2011/08/25/GAE_micolog_with_Django_1.2_enable/), the [GAE price model is not so tough](/2011/08/24/price-compare-sina-app-engine-vs-google-app-engine/) and Micolog is quite good for personal tech blog choose.

But after [GAE price policy changed](http://highscalability.com/blog/2011/9/7/what-google-app-engine-price-changes-say-about-the-future-of.html), I meet quite a lot Datastore Reads quota over Free budget, which lead almost the original blog inaccsible 3~4 hours everyday.

At same time, in the passed year, my favor language also change from Python to Ruby, so  to rebuild the blog using a Ruby language solution, the Octopress is more good choose for me.

The most unique feature of Octopress is it have no bandend DB require, all the blog content is purely static html file, which will be generate before deploy based on your markdown blog file. That make Octopress requirement very very minimal compare with Wordpress/Micolog. Basicly a static page site is already enough, so it make possible to just deploy this blog on [Github](https://github.com/Eric-Guo/Eric-Guo) directly.

Hoping for a long time, I'm not worry about the blog facility anymore.