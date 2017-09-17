---
layout: post
title: "Ruby Conf China 2017 Notes"
date: 2017-09-16T09:36:29+08:00
comments: true
external-url:
categories:
---

# 第一场 m-ruby on nginx from strikely

## Performance Result

目测删除了对m-ruby的结果，比如OpenResty的测试。

## XSS via blog using m-ruby

可以通过nginx层改json的格式，通过mruby语法，性能未知。

## Real usage in strikely

Image compress, jwt token verify

## Sample setup code

https://github.com/tylerdiaz/ngx-ruby

## Limitations

1.9.3 syntax only and can not require files and existing ruby gems.

# 第二场 Ethereum on Ruby

Rust和Ruby很有渊源，因为它们开头都是Ru。。。

区块链组成：加密算法、P2P网络、共识算法、可证实的数据结构（区块链 Authenticated Data Structure）。

Contract Account是的机器在历史上第一次财务独立，可以代替一切中间人的角色。

EVM Ethereum Virtual Machine 计费力度可以到指令。

交易一般在3秒内传遍世界，EVM跑交易，15秒出一个区块块。

Solidity, Viper（类Python，类型更安全）, Bamboo（做形式化证明）

Precompile Contract 预编译的合约用于加速处理，都是加密相关的功能。

# 第三场 Ruby异步编程奥德赛

全程无尿点，强烈推荐。

# 第四场 Erlang开发web框架

浪费一个小时。。

# 第五场 Ruby Web实时通讯方案剖析 侯俊杰

NChan方案。

# 第六场 Exploring ActiveRecord

# 第七场 Docker 发布

# 第八场 Ruby-Packer

Based on SquashFS

# 第九场 Elixir

# 第十场 金数据鉴黄

tesseract-ocr 字符识别

# 第十一场 mobx-ruby



