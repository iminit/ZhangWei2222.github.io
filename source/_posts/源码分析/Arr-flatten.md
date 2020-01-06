---
title: Arr-flatten
date: 2019-12-02 10:08:47
categories:
- 源码分析
comments: false
---



## 介绍

将多维数组一维化

<!-- more -->

## 使用

```js
$ npm install --save arr-flatten

var flatten = require('arr-flatten');
flatten(['a', ['b', ['c']], 'd', ['e']]); //=> ['a', 'b', 'c', 'd', 'e']
```



## 代码逻辑

1. 主函数`flat`里面递归，参数为：arr 和结果数组 res
2. 函数内循环遍历数组内的每个元素，if 元素是数组，那么递归`flat`，传入当前的元素及 res；else 把元素 push 到 res 内
3. 最后返回 res



## 学习

1. `var len = arr.length >>> 0;`无符号右移运算符，保证 len 有意义（为数字类型），且为正整数，在有效的数组范围内（0~0xFFFFFFFF）,且在无意义的情况下缺省值为 0
2. `+num` +号使字符串数字快速转成数字