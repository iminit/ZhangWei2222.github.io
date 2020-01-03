---
title: Slice.js
date: 2019-11-28 10:47:47
categories:
- 源码分析
comments: false
---



## 介绍

按照输入的参数截取数组或者字符串，参数可以是数字，或者字符 ‘开始:结尾:step’



## 使用

```js
$ npm install slice.js

import slice from 'slice.js';
// for array， string一样
const arr = slice([1, '2', 3, '4', 5, '6', 7, '8', 9, '0']);
arr[-2];  		// 9
arr['2:5'];  		// [3, '4', 5]
arr[':-2'];  		// [1, '2', 3, '4', 5, '6', 7, '8']
arr['-2:'];  		// [9, '0']
arr['1:5:2'];  		// ['2', '4']
arr['5:1:-2'];  	// ['6', '4']
```



## 代码逻辑

1. If 不为数组或者字符串，throw 出错误
2. 使用 Proxy 代理，重构 get 方法
   1. 检测输入的分隔符是否是数字，比如-2 或者 '2:5:3'
      1. if 是数字：正数，直接返回 v[n]，如果不是正数，返回 v[n+l] 即倒数第 n 个数。注意 n=2，输出的是 v[2]，n=-2，输出的是 v[l-2]，倒数第 2 个数，不太一样
      2. else 是字符串
         1. 需要先处理一下字符串：把字符如 ’2：4：2‘ 分割开来，弄成 start end step，没有传入的为 undefined 或 NaN（需要赋默认值）
         2. 把处理好的 start end step，传入 slice()，循环 push 进结果数组

## 学习

1. **new Proxy() 代理**

修改某些操作的默认行为，给目标对象之前架设一层拦截的es6 proxy对象，外部所有的访问都必须先通过这层拦截。

```js
var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(`getting ${key}!`);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(`setting ${key}!`);
    return Reflect.set(target, key, value, receiver);
  }
});
```

重新定义属性的获取和设置

2. **扩展运算符与解构结合**

```js
const r = slice(v, ...parseSliceString(path, l));
const parseSliceString = (path, l) => {
  return [start, end, step];
};
// 就依次能拿到start, end, step
const slice = (v, start, end, step) => {
};
```

