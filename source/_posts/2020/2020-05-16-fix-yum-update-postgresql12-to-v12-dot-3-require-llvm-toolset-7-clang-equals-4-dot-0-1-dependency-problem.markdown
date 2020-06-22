---
layout: post
title: "Fix yum update postgresql12 to v12.3 require LLVM-toolset-7-clang >= 4.0.1 dependency problem"
date: 2020-05-16T06:43:47+08:00
comments: true
external-url:
categories:
---

Simple running `yum update` on a CensOS 7 machine which installing the official postgresql v12 will get below problem:

```text
---> Package postgresql12-devel.x86_64 0:12.3-1PGDG.rhel7 will be an update
--> Processing Dependency: llvm-toolset-7-clang >= 4.0.1 for package: postgresql12-devel-12.3-1PGDG.rhel7.x86_64
--> Finished Dependency Resolution
Error: Package: postgresql12-devel-12.3-1PGDG.rhel7.x86_64 (pgdg12)
           Requires: llvm-toolset-7-clang >= 4.0.1
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

You can install CentOS SCLo RH repository and install `llvm-toolset-7-clang` to resolve it.

```bash
yum install centos-release-scl-rh
yum install llvm-toolset-7-clang
```
