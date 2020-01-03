---
title: Js 精度问题
date: 2019-11-06 18:13:37
categories:
- JavaScript
tags:
comments: false
---



## 前言

Js 浮点数运算会出现意想不到的结果：` 0.1+0.2=0.30000000000000000004`，可以使用一下方法进行精度计算



<!-- more -->

```js
/**
 * 小数加、减、乘、除 保留精度计算
 * @param {Number} value
 * @param {Number} decimals
 * for example
 * precisionRound(0.1 + 0.2, 2) 返回结果0.3
 */
export const precisionRound = (value, decimals) => {
  return Number(`${Math.round(`${value}e${decimals}`)}e-${decimals}`)
}
// 用公式
precisionRound(0.1+0.2, 2)
```