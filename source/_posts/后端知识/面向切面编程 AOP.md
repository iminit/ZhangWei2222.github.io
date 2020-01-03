---
title: 面向切面编程 AOP
date: 2020-01-02 16:26:37
categories:
- 后端知识
comments: false
---



## 前言

AOP 像 OOP 一样，只是一种编程范式，AOP 并没有规定说，实现 AOP 协议的代码，要用什么方式去实现。

<!-- more -->



## 是什么

1. AOP 即面向切面编程，是 OOP 编程的有效补充；
2. AOP 是 Spring 提供的关键特性之一；
3. AOP可以将那些与业务无关，却为业务模块所共同调用的逻辑或责任。比如事务处理、日志管理、权限控制等。封装起来，便于降低系统的反复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。 

![img](http://images.51cto.com/files/uploadimg/20100412/140615289.jpg)



## 相关概念

`Aspect 切面`：切入系统的一个切面。比如日志管理是一个切面，权限管理也是一个切面；

`Join point 连接点`：也就是可以进行横向切入的位置；

`Advice 通知`：切面在某个连接点执行的操作(分为: *前置通知 Before advice* , *后置返回通知  After returning advice* , *后置异常通知 After throwing advice* , *后置终于通知 After (finally) advice* , *围绕通知 Around advice* )；

`Pointcue 切点`：真正被切入的地方



## 分类

静态 AOP：指AspectJ实现的AOP，将切面代码直接编译到Java类文件中。

动态AOP：将切面代码进行动态织入实现的AOP。



Spring的AOP为动态AOP。