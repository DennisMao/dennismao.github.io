+++
title = "Proto3使用手记"
date = 2017-08-22T23:49:14+08:00
draft = false
slug = ""
categories = ["rpc","使用手记"]
tags = ["分布式","rpc"]
toc = true
+++

## Proto3 是什么？

  最近正在使用gRPC，当前版本gRPC使用的是google开发的Proto 3版本，因此顺带写一篇。Proto 3全称是 Protocol buffers v3.0,是谷歌公司开发的一款序列化数据结构。支持多种语言。与proto2相比主要有以下几点改变：
  
+ 原生支持更多语言 JavaNano,Go,Ruby,Objective-C和C#等
+ 调整部分语法 比如 default,optional,repeated 使得语法更简洁	
+ 增加部分特性
  
具体改变介绍推荐以下这篇文章  
[Protobuf 的 proto3 与 proto2 的区别](https://solicomo.com/network-dev/protobuf-proto3-vs-proto2.html)  
最新特性详情可查阅  
[Protobuf Release](https://github.com/google/protobuf/releases)  



## Proto3 有何功能?
通过定义接口描述文件 .proto文件，可通过这些描述自动生成出响应代码，用于编列或解码数据流。
## 为什么要选用它？
## 使用过程中需要注意什么？
## 使用过程

## 参考资料

[Proto3 开发文档](https://developers.google.com/protocol-buffers/docs/proto3)
