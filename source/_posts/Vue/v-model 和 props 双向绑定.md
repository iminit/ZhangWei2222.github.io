---
title: v-model 和 props 双向绑定
date: 2019-11-12 18:27:47
categories:
- Vue
comments: false
---



发现了一种数据双向绑定的好写法，子组件 v-model='item'，子组件用props接收 value。

一种语法糖，做了两个操作

1. v-bind绑定一个value属性
2. v-on指令给当前元素绑定input事件

```js
// 在自定义组件中
<my-component v-model="inputValue"></my-component>
// 相当于
<my-component v-bind:value="inputValue" v-on:input="inputValue = argument[0]"></my-component>

// 这个时候，inputValue接受的值就是input事件的回调函数的第一个参数，所以在自定义组件中，要实现数据绑定，还需要$emit去触发input的事件。
this.$emit('input', value)
```

