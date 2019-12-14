---
title: data不能使用箭头函数
date: 2019-10-11 09:04:47
categories:
- Vue
tags:
comments: false
---

 vue使用严格模式，this指向undefined，data的实现原理是每次创建新实例会判断是否有data函数，如果有会将其赋值给 vm._data

’如果是箭头函数，this 指向 undefined，会赋值给 vm._data，只要不刷新页面会缓存data，不使用箭头函数的话，this指向vm实例，会随着实例更新