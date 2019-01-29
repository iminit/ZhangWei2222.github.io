---
title: vue的v-if与v-show
date: 2017-09-09 15:15:47
categories:
- 前端技术篇
tags:
- VUE
- 饿了么
comments: false
---

### 1.共同点
都是动态显示DOM元素

### 2.区别
（1）手段：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐；

（2）编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；

（3）编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留；

（4）性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；

（5）使用场景：v-if适合运营条件不大可能改变；v-show适合频繁切换。

### Tips：
（1）如果v-show作用的元素，css文件中display:none，通过v-show进行设置不能显示该元素；
原因：v-show控制显隐，是通过js代码去修改元素的element style，如果value为false，设置display: none;如果value为true，设置display: ‘’；于是value为true时，只能将element style中的display效果清除，并不能覆盖css中的display效果；

如下图所示，value=true时，v-show改变的是element.style，由于无效，显示效果由css文件中的display决定。



### 解决办法：
使用v-show的话，在vue解析之前隐藏DOM的话，尽量在style属性里面设置display的值，不要在css文件中。


```
<ul v-touch:tap="message=2" style="display: none" v-show="show">
```

