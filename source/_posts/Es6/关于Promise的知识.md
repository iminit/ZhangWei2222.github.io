---
title: 关于Promise的知识
date: 2020-01-02 14:51:37
categories:
- Es6
comments: false
---



## 前言

看到一篇文章，[关于 Promise 的 9 个面试题](https://mp.weixin.qq.com/s/3K1TVRrVpKrqCIqRUeJSwA)，发现了知识盲点，记录一下

<!-- more -->



## 1. 多个`.catch`

```js
var p = new Promise((resolve, reject) => {
  reject(Error('The Fails!'))
})
p.catch(error => console.log(error.message))
p.catch(error => console.log(error.message))
// The Fails!
// The Fails!
```

**解析：**

使用**构造函数方法**创建一个 Promise ，并通过 `reject`回调立即触发错误。`.catch`工作方式类似于 DOM 的 `.addEventListener(event, callback)` 或 Event Emitter 的`.on(event, callback)`，**其中可以添加多个回调。**每个都用同样的参数进行调用。



## 2. 多个`.catch`

```js
var p = new Promise((resolve, reject) => {
  return Promise.reject(Error('The Fails!'))
})
p.catch(error => console.log(error.message))
p.catch(error => console.log(error.message))
// VM697:2 Uncaught (in promise) Error: The Fails!
```

**解析：**

使用 Promise 构造函数时，必须调用`resolve()`或`reject()`回调。

Promise 构造函数不使用自己定义的返回值，所以不会再收到由 `Promise.reject()` 创建的其他 Promise。



## 3. 链接`.then`和`.catch`

```js
var p = new Promise((resolve, reject) => {
  reject(Error('The Fails!'))
})
.catch(error => console.log(error))
.then(error => console.log(error))
// Error: The Fails!
// undefined
```

**解析：**

每个`.then`都接收一个`.then`返回的值作为其参数。但是如果步骤遇到错误，则任何后续的`.then`的步骤都将被跳过，直到遇到`.catch`。如果要覆盖错误，可以返回一个非错误值，可以通过任何随后的`.then`访问。



## 4. 链接`.catch`

```js
var p = new Promise((resolve, reject) => {
  reject(Error('The Fails!'))
})
.catch(error => console.log(error))
.catch(error => console.log(error))
// Error: The Fails!
```

**解析：**

当链接`.catch`时，每个仅处理先前的`.then`或`.catch`中引发的错误。在此例中，第一个`.catch`返回 `console.log`，只能通过在两个`.catch`之后添加`.then()`来访问。

```js
var p = new Promise((resolve, reject) => {
  reject(Error('The Fails!'))
})
.catch(error => {
  console.log(error); 
  return 1;
})
.then(error => {
  console.log(error); // 接收上面的返回值
  throw Error;
})
.catch(error => console.log(error)) // 接收上面的 Error
```



## 5. 多个`.catch`

```js
new Promise((resolve, reject) => {
  resolve('Success!')
})
  .then(() => {
  throw Error('Oh noes!')
})
  .catch(error => {
  return "actually, that worked"
})
  .catch(error => console.log(error.message))
// 不打印任何内容
```

**提示：**

`.catch`可以简单地返回一个常规值来忽略（或覆盖）错误。该技巧仅在随后的 `.then` 接收该值时有效。



## 6. `.then`之间的流程

```js
Promise.resolve('Success!')
  .then(data => {
  return data.toUpperCase()
})
  .then(data => {
  console.log(data)
})
// SUCCESS!
```

**提示：**

`.then`依次传递数据，从 `return value`到下一个`.then(value => /* handle value */)`，为了将值传递给下一个`.then`，`return`是关键

所以，以下例子就非常容易理解了

```js
Promise.resolve('Success!')
  .then(data => {
  return data.toUpperCase()
})
  .then(data => {
  console.log(data)
  return data
})
  .then(console.log(data))
// SUCCESS!
// SUCCESS!

Promise.resolve('Success!')
  .then(data => {
  data.toUpperCase()
})
  .then(console.log(data))
// undefined
```



## 7. `.then`和`.catch`之间的流程

```js
Promise.resolve('Success!')
  .then(() => {
  throw Error('Oh noes!') // 因为是resolve，执行 then ,抛出错误
})
  .catch(error => {
  return 'actually, that worked' // 接收 第一个then 抛出的错误，return 覆盖抛出的错误
})
  .then(data => {
  throw Error('The fails!') // 接收上面 第一个catch的 return ，又抛出错误
})
  .catch(error => console.log(error.message)) // 接收上面 第二个then 抛出的错误
// The fails!
```



## 总结

1. 使用 Promise 构造函数时，必须调用`resolve()`或`reject()`回调，后面可以添加多个回调。
2. `.then`可以接收 resolve 的回调；以及前一个`.then或.catch`的返回值，如果没有返回值，则是 undefined 。
3. 多个连续的链接`.catch`只会执行一个，接收 reject 回调，除非后面有所处理
4. `.catch`可以简单地返回一个常规值来忽略（或覆盖）错误。该技巧仅在随后的 `.then` 接收该值时有效。
5. `.then`依次传递数据，有`return`，就能接收。