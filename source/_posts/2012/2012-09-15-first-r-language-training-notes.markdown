---
layout: post
title: "The R language training notes"
date: 2012-09-15 11:04
comments: true
external-url:
categories: [R]
---
Very thanks to TopGeek and Capital of Statistics given this [R lecture](http://t.cn/zlPei2T), as part of Seven Languages in Seven Weeks, the [R language](http://w773.51qiangzuo.com/) is more unique than the [original Pragmatic book](http://pragprog.com/book/btlang/seven-languages-in-seven-weeks) language Prolog.

<!--more-->

##### Principle:

* Use matrix if possible
* Using functional programming style, only use parameter as data in function body
* Do not using global variable
* Do not using S4 (at least as begineer..)
* Use sapply as a map-reduce style

##### Tips:

1. Use data.frame rather than matrix as data.frame support more different type.
2. Use sqldf to select the data.frame
3. is.na() can detect NA as default value.
4. is.null() can detect NULL as empty value.
5. as.numberic() can convert any type to number type.
6. setwd("D:\\git"), setting working directory.
7. Keep control flow in program as simple as possible, e.g. only use if and for
8. set.seed(1) to have a determined start of random number

##### Some package recommand:

* stringr
* sqldf
* RODBC
* RPostgreSQL
* gdata
* XLConnect
* xml
* Rserve
* ggplot2