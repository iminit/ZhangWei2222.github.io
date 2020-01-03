---
title: Array-first
date: 2019-11-29 14:18:47
categories:
- 源码分析
comments: false
---



## 介绍

从头部截取指定个数的数组元素



## 使用

```js
$ npm install --save array-last

var first = require('array-first');
first(['a', 'b', 'c', 'd', 'e', 'f']); //=> 'a'
first(['a', 'b', 'c', 'd', 'e', 'f'], 1); //=> 'a'
first(['a', 'b', 'c', 'd', 'e', 'f'], 3); //=> ['a', 'b', 'c']
```



## 代码逻辑

1. If 不为数组，throw 出错误
2. If 数组长度为 0，返回 null
3. 把参数传入`slice库`，if 是数字或者字符数字，push（`slice库`实现的逻辑太繁琐，不如下面`array-last`简洁）
4. Else 默认截取头一位



## 学习

- 使用了 `+num`，因为代码中判断数字的逻辑中，无论是 number 还是 string，返回的是布尔值，需要把字符数字转变成数字
- 使用了`var len = arr.length >>> 0;`无符号右移运算符，保证 len 有意义（为数字类型），且为正整数，在有效的数组范围内（0~0xFFFFFFFF）,且在无意义的情况下缺省值为 0

