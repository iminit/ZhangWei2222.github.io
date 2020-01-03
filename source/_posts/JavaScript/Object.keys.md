---
title: Object.keys
date: 2019-11-01 18:24:37
categories:
- JavaScript
tags:
comments: false
---



处理对象，返回可枚举的属性数组

```js
let person = {
  name:"张三",
  age:25,
  address:"深圳",
  getName:function(){}
}
Object.keys(person) // ["name", "age", "address","getName"]
```