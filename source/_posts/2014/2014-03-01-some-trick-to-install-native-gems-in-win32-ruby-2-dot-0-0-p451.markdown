---
layout: post
title: "some trick to install native gems in win32 ruby 2.0.0-p451"
date: 2014-03-01T20:39:41+08:00
comments: true
external-url:
categories: [Ruby, Windows7]
---

I used to write [post about install ruby 2.0.0-p195 in windows](/2012/04/01/install-ruby-1-dot-9-3-and-rails-3-dot-2-3-on-windows-7/) one years ago, the ruby language improved a lot this year and it's gems. But windows platform is still not active compare with Mac OS X or Linux in rubyist.

There are two thick I believe worth to write down for the windows rubyist:

#### Install [debugger](https://github.com/cldwalker/debugger)

1. clone the [debugger-ruby_core_source](https://github.com/cldwalker/debugger-ruby_core_source) to local folder, e.g. `C:\git\debugger-ruby_core_source`
2. copy `C:\git\debugger-ruby_core_source\lib` to `C:\Ruby200\lib\ruby\gems\2.0.0\gems\debugger-ruby_core_source-1.3.2`
3. `gem install debugger`.

#### Install [bson](https://github.com/mongodb/bson-ruby)

Modify the win32.h file (@`C:\Ruby200\include\ruby-2.0.0\ruby`) before `install gems bson`

```c insert _PC_64 and _MCW_PC define
static inline double
rb_w32_pow(double x, double y)
{
    return powl(x, y);
}
#elif defined(__MINGW64_VERSION_MAJOR)

#ifndef _PC_64
#define _PC_64 0x00000000
#endif

#ifndef _MCW_PC
#define _MCW_PC 0x00030000
#endif

/*
 * Set floating point precision for pow() of mingw-w64 x86.
 * With default precision the result is not proper on WinXP.
 */
```