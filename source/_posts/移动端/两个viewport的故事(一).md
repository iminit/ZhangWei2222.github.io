---
title: 两个viewport的故事(一)
date: 2017-08-18 15:15:47
categories:
- 移动端
tags:
comments: false
---

### 一、设备像素和css像素
##### (1).设备像素dp(device pixels)
设备像素又称物理像素（physical pixel），设备能控制显示的最小单位，我们可以把这些像素看作成显示器上一个个的点。

##### (2).CSS像素
CSS像素是Web编程的概念，独立于设备的用于逻辑上衡量像素的单位，也就是说我们在做网页时用到的CSS像素单位，是抽象的，而不是实际存在的。

###### 设备独立像素比
独立于设备的用于逻辑上衡量像素的单位。（(比如：CSS 像素,只是在android机中CSS 像素就不叫”CSS 像素”了而是叫”设备独立像素”，两个基本一样）

不管是移动端还是PC端通过screen.width/height获取的这个值是设备独立像素（CSS 像素），而不是设备的屏幕分辨率

##### (3).PPI
PPI就是设备像素dp的单位

pixel per inch 翻译下就是每英寸内有多少个像素点,这个像素点指的是设备像素点（物理像素），说得接地气点PPI就是像素密度（pixel density）。



##### (4).设备像素比

```
设备像素比 = 设备像素/设备独立像素 // 在某一方向上，x方向或者y方向
```

可以通过JavaScript 中的window.devicePixelRatio来获取设备中的像素比值。

它可以告诉我们，一个设备像素对应多少个css像素



> 科普：什么是Retina视网膜屏幕？PPI 值超过 300 的叫做超高密度屏幕，只是 Apple 给它换了个高大尚的名称：Retina 视网膜屏幕而已。

##### (5).屏幕尺寸
意义：用户屏幕的整体大小。

度量单位：设备像素。

screen.width和screen.height



##### (6).窗口尺寸
意义：浏览器窗口的整体大小，包括滚动条。

度量单位：CSS像素。

它告诉了你用户到底有多少空间可以用来做CSS布局。你可以通过window.innerWidth和window.innerHeight来获取这些尺寸。



##### (7).滚动距离
意义：页面滚动的距离。

度量单位：CSS像素。



##### (8).viewport
viewport的功能是用来约束你网站中最顶级包含块元素（containing block）的。

viewport，接着，实际上等于浏览器窗口：它就是那么定义的。

##### (9).度量viewport
document.documentElement.clientWidth/Height

意义：Viewport尺寸。

度量单位：CSS像素。

不等于html的尺寸



##### (10).度量元素

```
document.documentElement.offsetWidth/Height
```


意义：元素（也就是页面）的尺寸。

度量单位：CSS像素。



##### (11).事件中的坐标
1. pageX/Y提供了相对于元素的以CSS像素度量的坐标。(IEb不支持)
1. clientX/Y提供了相对于viewport的以CSS像素度量的坐标。
1. screenX/Y提供了相对于屏幕的以设备像素进行度量的坐标。


90%的时间你将会使用pageX/Y；通常情况下你想知道的是相对于文档的事件坐标。其他的10%时间你将会使用clientX/Y。你永远不需要知道事件相对于屏幕的坐标。

##### (12).媒体查询
关于媒体查询的事。原理很简单：你可以声明「只在页面宽度大于，等于或者小于一个特定尺寸的时候才会被执行」的特殊的CSS规则。比如：


```
div.sidebar {
    width: 300px;
}

@media all and (max-width: 400px) {
    // styles assigned when width is smaller than 400px;
    div.sidebar {
        width: 100px;
    }

}
```

当前sidebar是300px宽，除了当宽度小于400px的时候，在那种情况下sidebar变得100px宽。

这个宽度是 viewport的宽高

width/height使用和documentElement .clientWidth/Height（换句话说就是viewport宽高）一样的值。它是工作在CSS像素下的。



