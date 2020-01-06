---
title: RPC
date: 2019-11-27 14:18:37
categories:
- 后端知识
tags:
comments: false
---

### 什么是RPC

RPC(Remote Procedure Call：远程过程调用)：一种进程间通信方式，允许像调用本地服务一样调用远程服务。

RPC框架负责屏蔽底层的传输方式（TCP或者UDP）、序列化方式（XML/JSON/二进制）和通信细节。主要目标就是让远程服务调用更简单、透明。开发人员在使用的时候只需要了解谁在什么位置提供了什么样的远程服务接口即可，并不需要关心底层通信细节和调用过程。

<!-- more -->

### RPC架构

- 客户端(client)：服务调用方
- 服务端(server)：服务提供方
- 客户端存根(client stub)：将客户端请求参数打包成网络消息，再发给服务方
- 服务端存根(server stub)：接收客户端发来的消息，将消息解包，并调用本地方法

![img](https://upload-images.jianshu.io/upload_images/11977583-e76bbbe6ecc1df4a.jpg)



### 与HTTP区别

- **RPC主要基于TCP/UDP协议**，HTTP协议是应用层协议，是构建在传输层协议TCP之上的，从效率来看的话RPC更胜一筹
- **RPC长连接**：不必每次通信都像http一样去三次握手，减少网络开销；
- HTTP服务开发迭代更快：在接口不多，系统与系统之间交互比较少的情况下，http就显得更加方便；相反，在**接口比较多**，系统与系统之间**交互比较多**的情况下，http就没有RPC有优势

#### 如何选择

一般来说，RPC服务主要是针对大型企业的，而HTTP服务主要是针对小企业的，因为RPC效率更高，而HTTP服务开发迭代会更快。



### 流行的RPC框架

- **gRPC**：Google 公布的开源软件，基于HTTP 2.0 协议

- **Thrift**： Facebook 的开源 RPC 框架，主要是一个跨语言的服务开发框架，基于TCP协议
- **Dubo**：阿里集团开源的一个Java RPC 框架，基于TCP协议

