---
title: NodeJs中的Eventloop
date: 2019-11-27 16:00:37
categories:
- 后端知识
tags:
- Node
comments: false
---

## NodeJs中的EventLoop图

<!-- more -->

```JS
   ┌───────────────────────┐
┌─>│        timers         │<————— 执行 setTimeout()、setInterval() 的回调
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
│  │     pending callbacks │<————— 执行由上一个 Tick 延迟下来的 I/O 回调（待完善，可忽略）
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
│  │     idle, prepare     │<————— 内部调用（可忽略）
│  └──────────┬────────────┘     
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
|             |                   ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │ - (执行几乎所有的回调，除了 close callbacks 以及 timers 调度的回调和 setImmediate() 调度的回调，在恰当的时机将会阻塞在此阶段)
│  │         poll          │<─────┤  connections, │ 
│  └──────────┬────────────┘      │   data, etc.  │ 
│             |                   |               | 
|             |                   └───────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
|  ┌──────────┴────────────┐      
│  │        check          │<————— setImmediate() 的回调将会在这个阶段执行
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
└──┤    close callbacks    │<————— socket.on('close', ...)
   └───────────────────────┘
```

1. 先执行所有类型为 timers 的 MacroTask，然后执行所有的 MicroTask（注意 NextTick 要优先哦）；
2. 进入 poll 阶段，执行几乎所有 MacroTask，然后执行所有的 MicroTask；
3. 再执行所有类型为 check 的 MacroTask，然后执行所有的 MicroTask；
4. 再执行所有类型为 close callbacks 的 MacroTask，然后执行所有的 MicroTask；
5. 至此，完成一个 Tick，回到 timers 阶段；
   如此反复，无穷无尽……



## 浏览器的EventLoop

```js
   ┌───────────────────────┐
┌─>│        timers         │<————— 执行一个 MacroTask Queue 的回调
│  └──────────┬────────────┘
|             |<-- 执行所有 MicroTask Queue 的回调
| ────────────┘
```

1. 先执行一个 MacroTask，然后执行所有的 MicroTask；
2. 再执行一个 MacroTask，然后执行所有的 MicroTask；
   如此反复，无穷无尽……



## 举一个栗子

```js
setTimeout(()=>{
    console.log('timer1')
    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)

setTimeout(()=>{
    console.log('timer2')
    Promise.resolve().then(function() {
        console.log('promise2')
    })
}, 0)
```

#### 浏览器环境下

![img](https://upload-images.jianshu.io/upload_images/2707400-2968b449856af912.gif?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

```js
timer1
promise1
timer2
promise2
```

#### NodeJs环境下

![img](https://upload-images.jianshu.io/upload_images/2707400-781ed56509d40758.gif?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

```js
timer1
timer2
promise1
promise2
```



## setTimeout 与 setImmediate 的顺序

在Node环境下，他们的执行顺序是不确定的。（顺便一提，浏览器永远是immediate先）

因为虽然 setTimeout 延时为 0，但是一般情况 Node 把 0 会设置为 1ms，所以，当 Node 准备 event loop 的时间大于 1ms 时，进入 timers 阶段时，setTimeout 已经到期，则会先执行 setTimeout；反之，若进入 timers 阶段用时小于 1ms，setTimeout 尚未到期，则会错过 timers 阶段，先进入 check 阶段，而先执行 setImmediate

```js
setTimeout(() => {
  console.log('timeout')
}, 0)

setImmediate(() => {
  console.log('immediate')
})
```

想要固定顺序，可以这样

```js
const fs = require('fs')

fs.readFile('test.txt', () => {
  console.log('readFile')
  setTimeout(() => {
    console.log('timeout')
  }, 0)
  setImmediate(() => {
    console.log('immediate')
  })
})
```

此时 setTimeout 和 setImmediate 是写在 I/O callbacks 中的，这意味着，都是处于 poll 阶段，然后是 check 阶段，所以这时无论 setTimeout 到期多么迅速，都会先执行 setImmediate