---
title: 关于Node的几个小点
date: 2019-11-22 18:07:37
categories:
- 后端知识
tags:
- Node
comments: false
---



1. node作为中间层，是怎么衔接后端的？
   - 通过node中的http模块，向接口发起请求，可以在node中做一些数据的处理、接口的合并等等。
2. 如果前端是上传文件，node作为中间层，需要存储文件吗？这个上传整个流的传输过程。
   - node不用存储文件，通过流的特性可以将这个请求转发给后台。
3. 为什么一般是node+mongodb，是因为mongodb的非关系数据库特点还是非sql语句的写法或者其他？
   - mongodb是基于js脚本语言的，也可以说是非sql语句的写法，因为mongodb主要是通过调用api的形式完成数据库的操作。