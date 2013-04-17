---
layout: post
title: "Thought after JMP Scripting Language training"
date: 2013-04-17T22:40:29+08:00
comments: true
external-url:
categories: [JMP, R]
---

I learn quite a lot of R in last 4 months in my spare time by attending [a network based class](http://www.dataguru.cn/article-2614-1.html). Today I also got a chance to attend a official SAS JMP software course called "Introduction to the JMP Scription Language" by my employer (only one day), so I would like to record some my thought about R and JMP here.

The most amazing part of JSL script is the funtion `Expr`, `Insert Into` and `Name Expr`, the teacher introduce JSL(JMP Script Language) as object oriented language, but using the three function mentioned before, I think the JSL is primarily a functional language instead, you building a new expression in the beginning of the program, modify the expression in the mid based on the condition or iteration way, then at the end of JSL script, evaluate the whole program as a expression.

So the JSL and R in the language level, share a lot about theoretical language feature/design priciple, although there is also quite a lot syntax detail difference.

On the other hand, the JMP and R design priciple in User Interface part is totally different, JMP is a totall interactive, click-select-OK-repert windows application while the R is even no GUI windows by default.

So JMP and R is the best gay friend in each other in my option, you start explore and found thing in JMP, after have a more clear/stable solution, you go using R as a implement tools, so you got the best part of each other.

JMP for its excellent UI and interactive workflow; R for its totally free license and be able to [export it's program as a web application](http://www.rstudio.com/shiny/).