---
title:  Chrome Devtool Performance 使用指南
date: 2019-12-20 11:28:37
categories:
- 浏览器
tags:
- Chrome Devtool
comments: false
---



## 前言

还记得距离上一次学习`Devtool`功能时，是在大二上学期，那会还叫`profile`。许久未见，`Devtool`强大了不少，今天探究一下`全新 Chrome Devtool Performance 的使用`。



`Performance`作用：分析运行时性能表现，可以根据提示去改善我们的代码，提高性能



## 测试

1. 匿名模式下，打开[DEMO](https://link.zhihu.com/?target=https%3A//googlechrome.github.io/devtools-samples/jank/)

2. 打开`Devtools`，点击`Performance`
   1. 确保 `Screenshots checkbox` 被选（打开截图功能）
   2. 点开右边的`小齿轮`，我们可以选择模拟 4倍的 CPU
   
3. 点击录制按钮（黑点），开始记录此段时间内页面运行性能

4. 点击`stop`停止

   

## 面板功能分析

![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/W.mNrPV.QixB5ylmRbiWvjxqWkOa9dEcaMGGWkdFkGs!/b/dLYAAAAAAAAA&bo=gAcYAwAAAAARB6w!&rf=viewer_4)



### 区域一：控制面板

- `Screenshots`截图：默认勾选，每一帧都会截图

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1eJ5sMBo7Xj*nyw.lEe2CaDqZCPn*j..lmKpwDJdMIYKLH393ygkgqklL3pvHFctWvyLysk.X1P3QsTPE9bsklA!/b&bo=6wJrAAAAAAARF6I!&rf=viewer_4)

- `Memory`内存消耗记录：勾选后可以看到各种内存消耗曲线

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1bGVReoIgCBr1Gg73Gc1M7*01AZ89rj6Ks*QQhnNqQcMOmok800Azr8kaIDXJTl5QSAs0LCAaF*Y*ijKHN.rofo!/b&bo=*QKkAAAAAAARF3s!&rf=viewer_4)



【以下配置是用来模拟手机、慢网络下使用】

- `Disable javaScript samples`关闭javaScript样本：减少在手机运行时的开销，模拟手机运行时勾选
- `Network`网络模拟：可以模拟在3G（自定义）等网络条件下运行页面
- `Enable advanced paint instrumentation(slow)`记录渲染事件的细节：选中后，选择frames中的一块，可以看到区域四多了`Layers`

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1f*JmWluI0iOm64*WBI4yBm*DLRdYsx*76yu9HKwEbka3Lx1DP0y9tq215aLGggLBK7*QdS008.G8umlMkyTbyI!/b&bo=gAflAQAAAAARB1M!&rf=viewer_4)

- `CPU`限制：模拟底CPU下的运行性能



### 区域二：概览面板


![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/tt6PHe6eDgduq05yrMrb3xBXSbuBUDriOF6xGSB8h4g!/b/dDYBAAAAAAAA&bo=OAFMAAAAAAARB0U!&rf=viewer_4)


#### 1. FPS

> 每帧秒数，对于动画而言标准是保持在60FPS
>
>FPS（frames per second）：用来分析动画的一个主要性能指标，让页面效果能够达到 >=60fps(帧)/s的刷新频率以避免出现卡顿。能保持在60的FPS的话，那么用户体验就是不错的。
>
> **为什么是60FPS呢？**
> 
> 因为这和目前大多数显示器的刷新率相吻合（60Hz），如果网页动画能够做到每秒60帧，就会跟显示器同步刷新，达到最佳的视觉效果。这意味着，一秒之内进行60次重新渲染，每次渲染的时间不能超过16.66毫秒



**分析**： 绿色越高越好，出现红色说明FPS低——页面卡顿的位置，可以在区域三的`Frames`中看到具体的FPS值





![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/2kfwdreq3l2Zgr5dTNPtc8ytIgL3iZOtdK77CO.wqeY!/b/dL8AAAAAAAAA&bo=jAGqAAAAAAARFwc!&rf=viewer_4)

#### 2. CPU

> 处理各个任务花费的时间，选择一段CPU统计可以在区域四的`Summary`看到统计表格

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1SUwjGnb1UOv.h3H01ZD7tqmqgmgBpWBSRCpEXmmpQBqRmrICYs7qwKRYwveMmMHKhaLt4yD8Bhd2tO0jG3NjZ4!/b&bo=gAcAAgAAAAARF6U!&rf=viewer_4)

- `Scirpting`脚本
- `Rendering`渲染
- `Painting`绘制
- `Loading`加载
- `ldle`闲置

**分析**：比重占的大的颜色可能有问题，着重注意



#### 3. NET

> 各个请求花费的时间，可以在区域三的`Network`里查看 网络请求详细请求

![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/4HrUPDOzzDm1mBbYm9tliYfH8FJdiTL2.Kr2hQutZ3c!/b/dL8AAAAAAAAA&bo=gAfYAAAAAAARF38!&rf=viewer_4)

**分析**：每条彩色横杠表示一种资源。横杠越长，检索资源所需的时间越长。每个横杠的浅色部分表示等待时间（从请求资源到第一个字节下载完成的时间）



### 区域三：线程面板

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1VsHmu9JZFmAgZFxMVnx3R6pAzVzBu9IzZHmjXUHK9aatRshiKZS7hCapMeBmZwEri09YGQiFNT3uM2n9wrVJlE!/b&bo=gAfTAAAAAAARB2Q!&rf=viewer_4)

#### 1. Frames

帧线程，鼠标悬浮绿色块可以看到FPS

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1WpNBhaZtb58.Ltd.yDZJKtH9.yQ.ZSjzkj6pcvvdSiAg7nZFOMoBOXMK3WzOgqri6QN1wsm4byixZN22ZFKiVY!/b&bo=UgK1AAAAAAARF8U!&rf=viewer_4)



#### 2. Main

主线程，负责执行javaScript，解析HTML/CSS，完成绘制。可以看到主线程调用栈和耗时情况，每个长条都是一个事件，悬浮可以看到耗时和事件名

- x轴：时间
- y轴：调用栈，上面的event调用下面的子event，越到下面数量越少（瀑布，倒立的火焰图）

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1Qg.nnSaYyrykBmf7JT*F680fxOBxdedDkY0SMIm7nlkysIad*ZqZbXBst8iYExv1nl13d1.mRhGfaC8tqoDEzk!/b&bo=rAOjAAAAAAARFyw!&rf=viewer_4)

颜色代表各个事件类型（更多可看**附录一**）

![img](http://a4.qpic.cn/psb?/V11Hmr5b1QVBmu/Z8L.tMfmaDjri7eR.ddhRSx2mTUU.gdUMQ4qLoKqerw!/b/dMMAAAAAAAAA&ek=1&kp=1&pt=0&bo=iwJqAQAAAAARF8I!&tl=3&vuin=985983437&tm=1577070000&sce=60-2-2&rf=viewer_4)



**分析**：

1. 在事件长条的右上角，如果出现了红色小三角，说明这个事件是存在问题的，需要特别注意

2. 双击带有红色小三角的事件，在`Summary`面板可以看到详细信息，注意`reveal`这个链接，双击它会让高亮触发这个事件的event。如果点击了app.js:94这个链接，会跳转到对应的代码处

   ![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/U8TeDa9n6Id8Y6umkx.G*l7HD8GxE*gxQmks.IX6mwI!/b/dFQBAAAAAAAA&bo=pAGFAQAAAAARBxE!&rf=viewer_4)

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1elOHRzxG0ABUTBWaCpVWhu2U4UJx0wUQtNOrdfM3iDpJPB0jxJ3jGq.EkD.qS1vGAWfrkyM*h5Wd5Ckam.aczk!/b&bo=tAA.AQAAAAARB7s!&rf=viewer_4)

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1SxQgLtftVALuAC8dC*.vcZ6zJwOBrRf*kSzIKVBfmmMiPiB1vyrdf0uyW2KRS9OZpnNrH7yq5tw97M0z3PbWtc!/b&bo=2QCAAQAAAAARF3g!&rf=viewer_4)

1. 在app.update这个事件的长条下方，有很多被触发的紫色长条，如果放大它们，可以看到每个都带有红色小三角。点击其中一个紫色事件长条，Summary面板展示了更多关于这个事件的信息
2. 在Summary面板里点击app.js:70链接，就会自动跳转到需要优化的代码处

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1ZnXH7FSm4Tdvqd7pFvT503GsiRBXEt2IwrEo*hz0RhwpMmlMY.sCaaq8A4OX.Byq4tRgE5..5mm8bB.ynTh0rM!/b&bo=jAE6AQAAAAARF5Y!&rf=viewer_4)

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1UR2MakK4Y5wX9LK7r6SrlBcUrhfP1OfAnO1TUeky1YvkGz3ZhdTQoFJc4kkfBlPsFp8gcEV2jhpvuS.bHhhoNE!/b&bo=dAMxAgAAAAARF2Q!&rf=viewer_4)



#### 3. Raster

Raster线程，负责完成某个layer或者某个块的绘制

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1WrYox9AsUFfsWjIagKlHkOmKTH9BSgPuc7iiAVaTRfHTD432LG.2Dfqq09UapXDjPG*.hNir7fODCLefZjhSRI!/b&bo=KQE9AAAAAAARFzU!&rf=viewer_4)



### 区域四：统计面板

统计面板会因点击选择不同的目标，统计的内容不同

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1W8DlzHQ6W6bea.YGvI0eU*bUQpx5WEq5Yt.CDgg0kIt4.nqhGRqhRsjRbG0Tpc4LB*q.x3gWbZpPJv.aeiRt*g!/b&bo=xQH.AAAAAAARFxo!&rf=viewer_4)

- `Summary`统计图：展示各个事件阶段耗费的时间

- `Bottom-Up`排序：事件时长排序
  
  - `self-time`：除去子事件这个事件本身消耗的时间
  
  - `total-time`：这个事件从开始到结束消耗的时间（包含子事件）
  
  ![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1a3locyqfTK5p*53zSEbyDJboH0jJdRmVm*GWJ4h241CVKHcPI*8jbDpVZ9IH34RrdinejncrH82ol14j8DY8I8!/b&bo=4QEzAQAAAAARF*I!&rf=viewer_4)
  
- `Call Tree`调用栈：事件调用排序，`Main`中选择一个事件，可以看到整个事件的调用栈（从最顶层到最底层，而不是只有当前事件）

  ![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1V.ia1iwwoXejm20t2hJOtrlWdrjWUGPqYL1FAa61sUHLOLMZGNLrrm5HbQ6VF7luDIlWPYpgzx6eGOdLdvQc6o!/b&bo=4AHMAAAAAAARFw0!&rf=viewer_4)

- `Event Log`事件日志：事件发生的先后顺序
  
  - 多了一个`start time`，指事件在多少毫秒开始触发的
  - 右边有事件描述信息
  
  ![img](http://m.qpic.cn/psb?/V11Hmr5b1QVBmu/B*y1EnRs*O8gp1DMn*FRht.IKQkkPXRyt5itzAwgLzs!/b/dLYAAAAAAAAA&bo=gAc0AQAAAAARF5I!&rf=viewer_4)



## 开启实时监控

1. `ctrl+shift+p`打开命令行
2. 搜索`Show Rendering`
3. 勾选`FPS Meter`

![img](http://m.qpic.cn/psc?/V11Hmr5b1QVBmu/jASqEt644m*KNERKGtvN1ZE8ja8nLaCvTULAzIOTdKGrP8EAIfFIy7Pz0ye8TEp6FpZ9sShbW5m3epZLMmIQKsaKM8F9Vcp3d01NxdMd95I!/b&bo=UgHpAAAAAAARF5o!&rf=viewer_4)



## 附录一

#### 1. Loading事件

| 事件             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| Parse HTML       | 浏览器执行HTML解析                                           |
| Finish Loading   | 网络请求完毕事件                                             |
| Receive Data     | 请求的响应数据到达事件，如果响应数据很大（拆包），可能会多次触发该事件 |
| Receive Response | 响应头报文到达时触发                                         |
| Send Request     | 发送网络请求时触发                                           |



#### 2. Scripting事件

| 事件                    | 描述                                                 |
| ----------------------- | ---------------------------------------------------- |
| Animation Frame Fired   | 一个定义好的动画帧发生并开始回调处理时触发           |
| Cancel Animation Frame  | 取消一个动画帧时触发                                 |
| GC Event                | 垃圾回收时触发                                       |
| DOMContentLoaded        | 当页面中的DOM内容加载并解析完毕时触发                |
| Evaluate Script         | A script was evaluated.                              |
| Event                   | js事件                                               |
| Function Call           | 只有当浏览器进入到js引擎中时触发                     |
| Install Timer           | 创建计时器（调用setTimeout()和setInterval()）时触发  |
| Request Animation Frame | A requestAnimationFrame() call scheduled a new frame |
| Remove Timer            | 当清除一个计时器时触发                               |
| Time                    | 调用console.time()触发                               |
| Time End                | 调用console.timeEnd()触发                            |
| Timer Fired             | 定时器激活回调后触发                                 |
| XHR Ready State Change  | 当一个异步请求为就绪状态后触发                       |
| XHR Load                | 当一个异步请求完成加载后触发                         |



#### 3. Rendering事件

| 事件              | 描述                            |
| ----------------- | ------------------------------- |
| Invalidate layout | 当DOM更改导致页面布局失效时触发 |
| Layout            | 页面布局计算执行时触发          |
| Recalculate style | Chrome重新计算元素样式时触发    |
| Scroll            | 内嵌的视窗滚动时触发            |



#### 4. Painting事件

| 事件             | 描述                                 |
| ---------------- | ------------------------------------ |
| Composite Layers | Chrome的渲染引擎完成图片层合并时触发 |
| Image Decode     | 一个图片资源完成解码后触发           |
| Image Resize     | 一个图片被修改尺寸后触发             |
| Paint            | 合并后的层被绘制到对应显示区域后触发 |