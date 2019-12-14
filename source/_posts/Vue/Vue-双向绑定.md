---
title:  Vue-双向绑定
date: 2019-03-18 15:14:37
categories:
- Vue
tags:
comments: false
---

## 前言 

Vue 是采用 数据劫持 结合 发布者-订阅者模式的方法，通过 Object.definedProperty() 来劫持各个属性的 setter、getter，在数据变化时发布消息给订阅者，触发相应的监听回调

<!-- more -->

## 具体步骤

第一步：需要 observe 的**数据对象进行递归遍历**，包括子**属性对象的属性**，**都加上 setter和getter**。这样的话，给这个对象的某个值赋值，就会触发 **setter**，那么就能**监听到了数据变化**



第二步：**compile解析模板指令**，将模板中的**变量替换成数据**，然后初始化渲染页面视图，并将**每个指令对应的节点绑定更新函数**，**添加**监听数据的**订阅者**，一旦数据有变动，收到通知，更新视图



第三步：Watcher 订阅者 是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

1、在自身实例化时往属性订阅器**(dep)**里面添加自己

2、自身必须有一个 **update()** 方法

3、待属性变动 dep.notice() **通知**时，能调用自身的 **update**() 方法，并**触发 Compile中绑定的回调** 



第四步：**MVVM **作为数据绑定的入口，**整合 Observer、Compile 和 Watcher三者**，通过 Observer 来监听自己的model数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果