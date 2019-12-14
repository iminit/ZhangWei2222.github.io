---
title: for in 与 for of
date: 2019-10-11 09:30:37
categories:
- JavaScript
tags:
comments: false
---

for in  循环返回的值是键名（数组的索引），for of返回的是 值



for in 还会遍历原型上的值和手动添加的其他键，会以任意顺序遍历，更适合遍历对象



for of 不会遍历原型上和手动添加的，可以打破循环



详细：http://www.fly63.com/article/detial/1444