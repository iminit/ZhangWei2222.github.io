---
title: Js异步任务macrotask和microtask
date: 2019-07-25 14:42:37
categories:
- JavaScript
tags:
- 运行机制
comments: false
---

## 前言
在阅读 Vue-nextTick源码时，看到有通过MutationObserver去实现nextTick（使用textnode模拟DOM变化，MO监听到后触发回调函数），百思不得其解：为什么可以使用MO这个API，为什么MO要优选与setTimeout？

<!-- more -->

## Js运行机制（刷新三观）

众所周知，JS是单线程的，主线程拥有一个执行栈和一个任务队列

主线程依次执行同步代码

当异步任务有结果时，把事件放入任务队列

当主线程的同步代码执行完毕了，又会依次地从任务队列取事件，执行异步任务的回调——event loop（事件循环）

 **但是重点来了**

任务队列分成tasks 和 microtasks，在每次事件循环中，tasks只会取一个执行，执行完成会检查microtasks队列，并把里面所有的任务都执行完毕。



## API

### macrotasks(tasks)
- setTimeout
-  setInterval
-  setImmediate
-  I/O
-  requestAnimationFrame
-  UI rendering

### microtasks
- process.nextTick,
- Promise
- MutationObserver
- Object.observe

```javascript
console.log(1);
setTimeout(function(){
  console.log(2);
}, 0);
Promise.resolve().then(function(){
  console.log(3);
}).then(function(){
  console.log(4);
});
结果：1 3 4 2 (promise比setTimeout优先级高)
```



## 注意

根据HTML Standrad，在每个task运行完以后，UI都会重新渲染，那么在microtask完成数据更新，当前task结束后就能得到新的UI；反之新建一个task来做数据更新，UI就会渲染两次



## 总结

现在我们就可以回答前言提到的两个问题啦：
- 为什么可以使用MO这个API
	- 它属于一个异步任务，到数据发生变化(Js主线程执行了赋值操作)，就会调用起microtasks任务队列的任务MO(MO做了啥请移动到 Vue-nextTick)
- 为什么MO要优选与setTimeout
	- 因为MO是一个microtask，setTimeout是一个task，task做数据更新，UI会渲染两次，耗费性能



## 补充

```js
// CVTE笔试题2019.09.05
process.nextTick(function(){
    console.log(1);
    process.nextTick(function(){
        console.log(2);
    })
})
setTimeout(function(){
  console.log(4);
}, 0);
process.nextTick(function(){
    console.log(3);
})
console.log(5);
// 5 1 2 3 4
//如果有多个process.nextTick语句（不管它们是否嵌套），将全部在当前"执行栈"执行
```

