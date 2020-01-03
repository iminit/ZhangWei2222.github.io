---
title:  Chrome Devtool Memory 使用指南
date: 2020-01-03 09:46:37
categories:
- 浏览器
tags:
- Chrome Devtool
comments: false
---



## 前言

`Memory`作用：方便开发人员定位内存泄漏问题，从 内存 着手，提高页面性能

<!-- more -->



## Chrome 任务管理器

Chrome 设置 -> 更多工具 -> 任务管理器 -> 右键任务管理器的表格标题处，启用 JavaScript 使用的内存

![img](https://img.youpin.mi-img.com/jianyu/8e3f2cda1049bd29a5f53df2050bdfd9.jpeg?w=1864&h=1044)

- **内存占用空间：原生内存。**DOM 节点存储在原生内存中，如果此值正在增大，说明正在创建 DOM 节点
- **JavaScript 使用的内存：JS堆。**包含两个值，关注实时数字（括号中的数字），实时数字表示页面上的可到达对象正在使用的内存量。如果此数字在增大，要么是正在创建新对象，要么是现有对象正在增长



## Memory 面板

![image-20200103095746916](/Users/wei/Library/Application Support/typora-user-images/image-20200103095746916.png)

- `Heap snapshot`：打印堆快照，堆快照文件显示页面的 JavaScript 对象和相关 DOM 节点之间的内存分配
- `Allocation instrumentation on timeline`：在时间轴上记录内存信息，随着时间变化记录内存信息
- `Allocation sampling`：内存信息采样，使用采样的方法记录内存分配。此配置文件类型具有最小的性能开销，可用于长时间运行的操作。它提供了由 JavaScript 执行堆栈细分的良好近似值分配，哪些函数影响了内存的分配，并且该函数所耗内存在内存分配中占比多少



## `Heap snapshot`

![img](https://img.youpin.mi-img.com/jianyu/779dc7e0f200d1e4a74c2a2f797230d7.jpeg?w=1918&h=324)

**区域一：**

从左到右有三个操作：查看方式、对象归类的筛选、快照选择

查看（内存快照）方式有：

- `Summary`：可以显示**按构造函数名称分组**的对象。使用此视图可以根据按构造函数名称分组的类型，深入了解对象及其内存使用，适用于跟踪 DOM 泄漏
- `Comparison`：可以**显示两个快照之间的不同**。使用此视图可以比较两个（或多个）内存快照在某个操作前后的差异。检查已释放内存的变化和参考计数，可以确认是否存在内存泄漏以其原因
- `Containment`：此视图提供了一种**对象结构视图**来分析内存使用，由顶级对象作为入口
- `Statistic`：内存使用饼状的**统计图**

**区域二：**

- `Shallow Size`：浅层大小，对象自身占用的内存大小
- `Retained Size`：保留大小，将对象本身联通其无法从 GC 根到达的相关对象一起删除后释放的内存大小
- `Distance`：使用节点到根节点最短路径距离



### Comparison 视图

比如：

```vue
<template>
<div class="hello">
  {{testArray}}
  <div @click="click">增加</div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        testArray: [{ value: "hello" }]
      };
    },
    methods: {
      click() {
        this.testArray.push({
          value: ":::::::::"
        });
      }
    }
  };
</script>
```

点击 增加 按钮前，打印一下堆快照；点击 增加 按钮后，打印一次堆快照，选择 Comparison 视图，会与快照 1 进行比较


![img](https://img.youpin.mi-img.com/jianyu/85a96443003d9d1d22120a79f8e03a8c.jpeg?w=1919&h=692)

**分析：**Retainers 就是这个值的保留树，表明这个值存储在内存位置为（345883）的 VNode 的 text 键上，这个 VNode 又位于 Array （345887）的 0号位上，Array 是一个 children 位于 VNode 上



#### 闭包（closure）

![img](https://img.youpin.mi-img.com/jianyu/ad22ebcdb9bed7bca90cf342143fd807.jpeg?w=1920&h=573)

闭包可以通过一对正反操作进行比较，比如点击造成了一个闭包，再点击一次释放一个闭包，查看闭包所占内存。闭包情况很常见，一般情况下只要闭包没有滥用就不会有什么问题



### Containment 视图

![img](https://img.youpin.mi-img.com/jianyu/599b9682bb9543ebd76ed881d7e63c69.jpeg?w=1721&h=296)

- `Distance`：从 window 出发的最短保留路径上的属性引用数量

- 文档 DOM 树：由可以通过遍历文档到达的所有原生 DOM 节点组成

内存图从根开始，根可能是浏览器的 window 对象或者 Node 的 Global 对象，我们无法控制此根对象的垃圾回收方式。任何无法从根到达的对象都会被 GC 回收

Size 指 Js 堆，原生对象不会在 Js 堆中表示，所以大小为 0



## `Allocation instrumentation on timeline`

比如：

```js
click() {
  // 每次点击 字符串长度都以上一次为基础增加到5倍，拉大差异突出效果，并且之后在字符串头部加上count值做区分
  this.count *= 5;
  var str = new Array(this.count * 10).join(":");
  this.testArray.push({
    value: this.count + str
  });
}
```


![img](https://img.youpin.mi-img.com/jianyu/fbbb1008441917972082060fdd8b2d00.jpeg?w=1057&h=314)

![img](https://img.youpin.mi-img.com/jianyu/14ffa267456367d7d851df94d9d38e59.jpeg?w=1729&h=703)

- 每条线的高度与最近分配的对象大小对应

- 竖线的颜色：这些对象是否仍然显示在最终的堆快照中
  - 蓝色竖线：在时间线最后对象仍然显示
  - 灰色竖线：对象已在时间线期间分配，但曾对其进行垃圾回收




## `Allocation sampling`

![img](https://img.youpin.mi-img.com/jianyu/9b4c3c992b535d7820c3c2af8acc4c32.jpeg?w=1920&h=683)

可以通过 Chart、Tree、Heavy 不同的形式，查看函数调用栈、函数占用的内存、引用内存等，点击可以跳转到具体的 source 代码处