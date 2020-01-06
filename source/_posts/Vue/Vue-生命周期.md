---
title:  Vue-生命周期
date: 2019-03-18 10:58:37
categories:
- Vue
tags:
comments: false
---

## 生命周期

- beforeCreate（创建前）
- created（创建后）
- beforeMount(载入前)
- mounted（载入后）
- beforeUpdate（更新前）
- updated（更新后）
- activated （keep-alive 激活用）
- deactivated（keep-alive 移除用）
- beforeDestroy（销毁前）
- destroyed（销毁后）

<!-- more -->

#### keep-alive

Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染dom

- include: 字符串或正则表达式。只有匹配的组件会被缓存。
- exclude: 字符串或正则表达式。任何匹配的组件都不会被缓存。



## 父子组件的生命周期顺序

#### 加载渲染过程

```js
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted 
```



#### 子组件更新过程

```js
父beforeUpdate->子beforeUpdate->子updated->父updated 
```



#### 父组件更新过程

```js
父beforeUpdate->父updated 
```



#### 销毁过程

```js
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed 
```

