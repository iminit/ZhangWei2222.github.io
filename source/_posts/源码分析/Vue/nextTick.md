---
title: Vue-nextTick源码浅析
date: 2019-07-24 18:11:47
categories:
- 源码分析
tags:
- Vue
comments: false
---

## 前言

**nextTick** 的作用： 让回调函数在DOM更新完成后被调用。

<!-- more -->

```javascript
举个栗子：
new Vue({
  el: '#app',
  data: {
    list: []
  },
  mounted: function () {
    this.get()
  },
  methods: {
    get: function () {
      this.$http.get('/api/article').then(function (res) {
        this.list = res.data.data.list
        // ref  list 引用了ul元素，我想把第一个li颜色变为红色
        this.$refs.list.getElementsByTagName('li')[0].style.color = 'red'
      })
    },
  }
})

结果报错，因为此时此刻dom上的list还没有数据，也就是说赋值操作没有引起视图的更新；需要改成：
this.list = res.data.data.list
this.$nextTick(function () {
	this.$refs.list.getElementsByTagName('li')[0].style.color = 'red'
})
```

## 原理
Vue的特点：响应式。但是数据更新时，DOM不会立即更新。因为Vue在更新DOM时是异步执行的，只要侦听到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个watch被多次触发，只会被推入到队列中一次(去除重复数据，避免不必要的计算和DOM操作)。当刷新(执行)队列时，DOM会在下一个事件循环‘tick'中更新。

## 使用
```javascript
import Vue from 'vue'
Vue.nextTick(function () {
  console.log(this.$refs.msg.innerHTML) // 已更新
})

//下面的方法不需要使用全局Vue，而且this将自动绑定到当前的Vue实例上
this.$nextTick(function () {
	console.log(this.$refs.msg.innerHTML) // 已更新
})
```

## 应用场景
- created()钩子函数进行的DOM操作一定要放在nextTick()的回调函数中	
	- 因为created() 时DOM还没有渲染，写在nextTick()中相当于mounted()函数，DOM的挂载和渲染已完成
- 如果某个操作，需要使用随数据改变的DOM结构时，需要放在nextTick()的回调函数中	

## 源码浅析

源代码在@vue/src/core/util/next-tick.js
定义变量：
```javascript
var callbacks = [];   // 存储回调函数的数组
var pending = false;  // 是否正在执行回调函数
var timerFunc;  // 触发执行回调函数
```
$nextTick内实际调用的函数：(不直接操作callbacks，是为了防止后续有新的回调函数操作传入)
```javascript
function flushCallbacks () {
  pending = false
  // 拷贝出回调函数数组副本
  const copies = callbacks.slice(0)
  // 把函数数组清空
  callbacks.length = 0
  // 依次执行函数
  for (let i = 0; i < copies.length; i++) {
​    copies[i]()
  }
}
```
接下来，分了4种情况来延迟调用flushCallbacks()(依次优雅降序)
### 1、Promise.then 延迟调用
```javascript
if (typeof Promise !== 'undefined' && isNative(Promise)) {
  const p = Promise.resolve()
  timerFunc = () => {
    p.then(flushCallbacks)
    if (isIOS) setTimeout(noop)
  }
  isUsingMicroTask = true
}
```
Promise.then为microtasks的一种异步任务，当主线程(数据赋值)完成后，才会调用Promise.then，从而做到延迟(详情看microtask和task的文章)

### 2、MutationObserver 监听变化
```javascript
if (!isIE && typeof MutationObserver !== 'undefined' && (
  isNative(MutationObserver) ||
  MutationObserver.toString() === '[object MutationObserverConstructor]'
)) {
  let counter = 1
  const observer = new MutationObserver(flushCallbacks)
  const textNode = document.createTextNode(String(counter))
  observer.observe(textNode, {
    characterData: true
  })
  timerFunc = () => {
    counter = (counter + 1) % 2
    textNode.data = String(counter)
  }
  isUsingMicroTask = true
} 
```
MutationObserve是HTML5的一个新API，功能是：监听dom节点变化，在所有dom变动完成后，执行回调函数。

MO也是一个microtasks，在这里是创建一个文本节点，当主线程(数据赋值)完成后，会改变文本节点的内容来触发MutationObserve，执行回调，从而达到延迟的作用。

### 3、setImmediate 延迟器
```javascript
if (typeof setImmediate !== 'undefined' && isNative(setImmediate)) {
  timerFunc = () => {
    setImmediate(flushCallbacks)
  }
} 
```

### 4、setTimeout 延迟器
```javascript
{
  timerFunc = () => {
    setTimeout(flushCallbacks, 0)
  }
```
setImmediate和setTimeout都是task中的异步任务类型，它们会等待主线程(数据赋值)完成后，再起新的一个task执行回调函数，也起到了延迟的作用。setImmediate比setTimeout优先级更高。

### 闭包函数
```javascript
export function nextTick (cb?: Function, ctx?: Object) {
  let _resolve
  callbacks.push(() => {
    if (cb) {
      try {
        cb.call(ctx)
      } catch (e) {
        handleError(e, ctx, 'nextTick')
      }
    } else if (_resolve) {
      _resolve(ctx)
    }
  })
  // 如果没有函数队列在执行才执行
  if (!pending) {
    pending = true
    timerFunc()
  }
  // promise化
  if (!cb && typeof Promise !== 'undefined') {
    return new Promise(resolve => {
      _resolve = resolve
    })
  }
}
```


每一次添加函数，都会向callbacks这个函数数组入栈。然后监听当前是否正在执行，如果没有，执行函数。

```javascript
this.$nextTick(function () {
})

// promise化
this.$nextTick().then(function () {
}.bind(this))
```