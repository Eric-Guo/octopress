---
layout: post
title: "Grid Computing answer for Python Code Dojo Class Two"
date: 2012-08-25 15:26
comments: true
external-url:
categories: [Python]
---

The answer for [Python Code Dojo Class Two](http://topgeek.org/?p=527), which I attend today as seven language in seven week hold by Shanghai TopGeek.

```python calculate the maximum value found by summing all the rows and columns of a grid of numbers
stin="""01 34 46 31 55 21 16 88 87 87
32 40 82 40 43 96 08 82 41 86
30 16 24 18 04 54 65 96 38 48
32 00 99 90 24 75 89 41 04 01
11 80 31 83 08 93 37 96 27 64
09 81 28 41 48 23 68 55 86 72
64 61 14 55 33 39 40 18 57 59
49 34 50 81 85 12 22 54 80 76
18 45 50 26 81 95 25 14 46 75
22 52 37 50 37 40 16 71 52 17"""

g=[map(int, str.rsplit(row)) for row in str.rsplit(stin, '\n')]

d={}

for idx, val in enumerate(g):
    d[sum(val)]="row %s"%(idx+1)

tg=zip(*g)

for idx, val in enumerate(tg):
    d[sum(val)]="col %s"%(idx+1)

kd=list(d.keys())
kd.sort(reverse=True)
print "Max value: %s: %s" % (d[kd[0]], kd[0])
```

The complete part of the class:

1. [Code Dojo Class One](http://topgeek.org/?p=526)
2. [Code Dojo Class Two](http://topgeek.org/?p=527)
3. [Code Dojo Class Three - An Intro to TDD and unittest of Python](http://topgeek.org/?p=537)