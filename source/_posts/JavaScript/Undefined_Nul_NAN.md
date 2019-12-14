---
title: undefined&null&NaN
date: 2019-04-09 15:32:37
categories:
- JavaScript
tags:
comments: false
---

### 数据类型的区别

undefined和null是两个基本类型



### 存在场景不同

undefined是未初始化变量的值

null通常用来标识一个空对象

typeof(undefined) ==undefined

typeof(null) ==object



### 判断undefined方法

typeof()



### 判断null方法

typeof(x) !='undefined' && !x && x!=0

(x为false有3种情况：0 undefined null 空字符串''等于0)



### 其他

10 + null = 10

10 + undefined = NaN


undefined == true //false

undefined == false //false

null == true //false

null == false //false