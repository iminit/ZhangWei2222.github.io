---
title:  Vue-填坑笔记
date: 2020-02-22 11:24:37
categories:
- Vue
tags:
- 填坑
comments: false
---



记录 Vue 开发中遇到的坑
<!-- more -->



1. Property or method "artificial1" is not defined on the instance but referenced during render. Make sure that this property is reactive, either in the data option, or for class-based components, by initializing the property.

- 子组件直接引用了props里的值，这样父组件改变值时子组件会报错
- 解决方法：将值data中复制一份，子组件引用复制的值



2. vue + ts 使用 `$refs` 调用函数报错

- 代码`this.$refs.a.toggle();`显示没有`toggle()`方法，但是打印是有的
- 解决办法：增加声明 `$refs: {quickEntry: HTMLFormElement}`

   

