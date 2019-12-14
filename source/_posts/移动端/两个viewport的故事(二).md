---
title: 两个viewport的故事(二)
date: 2017-08-18 15:15:47
categories:
- 移动端
tags:
comments: false
---

### 1.两个viewport
##### (1).visual viewport
visual viewport是页面当前显示在屏幕上的部分。用户可以通过滚动来改变他所看到的页面的部分，或者通过缩放来改变visual viewport的大小。



##### (2).layout viewport


##### (3).度量visual viewport


##### (4).滚动距离Scrolling offset


你还需知道的是visual viewport当前相对于layout viewport的位置。这是滚动距离，并且就像在桌面一样，它被存储在window.pageX/YOffset之中。

##### (5). 元素


就像在桌面上一样，document.documentElement.offsetWidth/Height提供了以CSS像素为单位的元素的整个尺寸。

##### (6).媒体查询Media queries


媒体查询和其在桌面环境上的工作方式一样。width/height使用layout　viewport做为参照物，并且以CSS像素进行度量，device-width/height使用设备屏幕，并且以设备像素进行度量。

换句话说，width/height是document.documentElement.clientWidth/Height值的镜像，同时device-width/height是screen.width/height值的镜像。（它们在所有浏览器中实际上就是这么做的，即使这个镜像的值不正确。）

##### (7).事件坐标


pageX/Y仍然是相对于页面，以CSS像素为单位，并且它是目前为止三个属性对中最有用的，就像它在桌面环境上的那样。

clientX/Y是相对于visual viewport来计算，以CSS像素为单位的。这有道理的，即使我还不能完全指出这么做的好处。

screenX/Y是相对于屏幕来计算，以设备像素为单位。当然，这和clientX/Y用的参照系是一样的，并且设备像素在这没有用处。所以我们不需要担心screenX/Y；跟在桌面环境上一样没有用处。

##### (8).viewport meta标签
