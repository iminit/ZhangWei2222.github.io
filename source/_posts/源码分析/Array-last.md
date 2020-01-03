---
title: Array-last
date: 2019-11-29 09:59:47
categories:
- 源码分析
comments: false
---



## 介绍

从尾部截取指定个数的数组元素



## 使用

```js
$ npm install --save array-last

var last = require('array-last');
last(['a', 'b', 'c', 'd', 'e', 'f']); //=> 'f'
last(['a', 'b', 'c', 'd', 'e', 'f'], 1); //=> 'f'
last(['a', 'b', 'c', 'd', 'e', 'f'], 3); //=> ['d', 'e', 'f']
```



## 代码逻辑

1. If 不为数组，throw 出错误

2. If 数组长度为 0，返回 null

3. If 输入的参数是 数字或者字符数字

   1. 定义辅助数组

   2. while 循环，赋值给辅助数组，最后返回

      ```js
      while (n--) {
        // n为输入的参数
        res[n] = arr[--len];
      }
      ```

4. Else 不是数字或者字符数字

   - 默认截取最后一位元素



## 学习

- `while`的用法

