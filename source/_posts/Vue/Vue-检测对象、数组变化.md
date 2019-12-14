---
title: Vue-检测对象、数组变化
date: 2019-06-17 14:28:47
categories:
- Vue
tags:
- Vue-进阶
comments: false
---

## 前言
使用Vue开发的过程中，有的时候操作变量的时候老是无法触发更新，比如以下几种情况
- 无法做到 **对象属性** 的添加/删除
- 无法做到 **数组** 的一些变化
	- 利用索引值直接设置元素：this.arr[0] = 'y'//不是响应的
	- 修改数组长度 this.arr.length = 5 //不是响应的
这是为什么呢？？

## 为什么无法做到对象属性的添加与删除
Vue使用Object.defineProperty，在初始化实例时对属性执行getter/setter转化，进行监测，属性必须在data对象上存在才能让Vue将它转换成响应式。

### 解决方案
```javascript
this.someObject.b = 2
改成：
Vue.set(this.someObject, 'b', 2)
this.$set(this.someObject, 'b', 2)

为已有对象赋值多个新属性
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

## 为什么不能监听数组的一些变化
如果监测的值是 **值** 类型，当值变化就会触发set；如果监测的值是 **引用** 类型，虽然值变了，但是引用没有变，所以不会触发set

### 解决方案
```javascript
Vue.set(vm.items, indexOfItem, newValue)
vm.$set(vm.items, indexOfItem, newValue)
Vue重写了一些方法，我们可以灵活使用：push, pop, shift, unshift,splice, sort, reverse
vm.items.splice(indexOfItem, 1, newValue)
vm.items.splice(newLength)
```

