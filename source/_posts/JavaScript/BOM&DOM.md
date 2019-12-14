---
title: BOM&DOM
date: 2019-04-17 15:32:37
categories:
- JavaScript
tags:
comments: false
---

### BOM & DOM
**BOM 浏览器对象模型**

有window、location、history、screen等对象

window常用的方法：

open()   打开一个新的资源。

moveTo() 将窗口左上角的屏幕位置移动到指定的 x 和 y 位置。

moveBy()  相对于当前的窗口移动指定的 x 和 y 偏移值(左上角)。

setInterval()  每隔指定的毫秒数指定指定的代码。

setTimeout（） 经过指定毫秒数指定一次指定的代码。 

**DOM 文档对象模型** 当一个html页面加载到浏览器的时候,那么浏览器会为每个标签都创建一个对应的对象描述该标签的所有信息，那么我们看到的网页信息实际上就是看到了这些标签对象的信息、 如果我们需要操作页面的数据，那么我们就可以通过这些标签对象进行操作。

### Global对象 &  window对象
**Global对象**

单体内置对象，即不依赖于宿主环境的对象，这些对象在ECMAScript程序执行之前就已经存在了，就是一切全局里存在的变量和函数都是它的属性和方法。也就是那些在宿主环境（常见的宿主环境：浏览器和nodejs）里所有的内建或自定义的变量和函数全局都是Global这个的全局对象和方法。它更像是一个抽象概念，而要指明它是什么，取决于程序在什么环境中运行。（例如：js不仅可以书写页面，在书写页面中，global相对于浏览器这个环境而言就是window，但是其他环境就不一定了）

**window对象**

是相对于Web浏览器而言的，它并不是ECMAScript规定的内置对象，它是浏览器的Web API,是存在于浏览器之中的，也就是离开浏览器这个宿主环境的话就不存在此对象了。

《JavaScript高级程序设计第3版》：在所有代码执行之前，作用域中就存在两个内置对象：Global和Math。在大多数ECMAScript实现中都不能直接访问Global对象，不过，Web浏览器实现了承担该角色的window对象。全局变量和函数都是Globla对象的属性。



**结论**JavaScript中的window对象扮演ECMAScript中的Global对象，所以Global对象包含着window对象