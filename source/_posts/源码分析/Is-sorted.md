---
title: Is-sorted
date: 2019-11-28 15:04:47
categories:
- 源码分析
comments: false
---


## 介绍

判断一个数组是否是顺序或者倒序的



## 使用

```js
$ npm install --save is-sorted

var sorted = require('is-sorted')
console.log(sorted([1, 2, 3])) // => true
console.log(sorted([3, 1, 2])) // => false
// supports custom comparators
console.log(sorted([3, 2, 1], function (a, b) { return b - a })) // => true
```



## 代码逻辑

1. If 不为数组，throw 出错误

2. 定义了一个默认的判断排序的方法：升序，可以传入自定义判断方法

3. 循环数组，调用判断方法，假如 不满足 则返回 false

   ```js
   // 默认判断方法
   function defaultComparator(a, b) {
     return a - b;
   }
   // 循环判断 不管是哪种判断方法，都是小的减大的，升序a-b，降序b-a，所以出现大于0的时候，就跳出，说明不是顺序排序
   if (comparator(array[i - 1], array[i]) > 0) return false;
   ```

