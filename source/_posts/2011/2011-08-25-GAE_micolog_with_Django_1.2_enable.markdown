---
layout: post
title: "A GAE based blog, Micolog 7.412 with Django 1.2 enable"
date: 2011-08-25 00:00
comments: true
external-url:
categories: [Life, Python, Cloud]
---
I would like to thank XuMing and his <a href="http://micolog.xuming.net/" target="_blank">Micolog</a>, it&rsquo;s very small blog program on GAE but functional is quite complete. Micolog is a good reference source code if you are newly to <a href="https://appengine.google.com/" target="_blank">Google App Engine</a>. There is also someone who write a article about how to <a href="http://blog.spbk.net/Micolog%E5%8D%87%E7%BA%A7%E9%BB%98%E8%AE%A40_96%E7%89%88%E6%9C%ACDjango%E8%87%B3Django1_2#reply" target="_blank">upgrade the exist Micolog from Django 0.96 to Django 1.2</a>, the article is download is broken and still have some problem about template safe notation modify here and there.

So I would like to contribute a modified version of <a title="Micolog.v7.412.zip" href="http://115.com/file/clq5d0rl#%20Micolog.v7.412.zip" target="_blank">Micolog v7.412</a> source code, which I spend quite several days to debug and merge the change both in the new <a href="https://github.com/xuming/micolog/commits/master" target="_blank">github.com</a> and old <a href="http://code.google.com/p/micolog/source/checkout" target="_blank">SVN on Google Code</a> code repository, I also change/enhanced the feature as below:

* Update tinymce editor to version 3.9.4
* Fix IE6 can not comment bug
* Fix sys_plugin can no load error
* Minor change Pixel theme css
* Add IE9 pin site feature in Pixel
* Do some python code standard layout and import optimize
* Allow suppress logo and other not needed copyright in admin page
* Fix email notify in sys_plugin after upgrade to 1.2
* Add replay via comments
* New paging facility

The source code is also used in this blog and seems quite stable and I hope you will like it.