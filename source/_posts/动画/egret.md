---
title: egret
date: 2018-08-01 21:42:35
categories:
- 动画
tags:
comments: false
---

# Get Started

Egret 采用 TypeScript 进行编程，它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。TypeScript 扩展了 JavaScript 的语法，原生 JavaScript 的方法在 TypeScript 也是可以使用的。比如Math等。

<!-- more -->

# 建立工作环境

Egret Wing 是一款针对 Egret 项目的集成开发环境，通过可视化的方式创建、编辑和管理项目中所有游戏用户界面，并可以同时进行 TypeScript 代码编写和断点调试。

在图形图像渲染中，Egret完全使用HTML5标准中的canvas技术,可以运行在支持 HTML5 的浏览器上。但是在我们调试项目的时候还是推荐使用 Chrome 浏览器，一个方面是对 HTML 5 支持的程度较高，二是可以方便的调试 JavaScript 和进行性能分析等。

#清理项目

打开Main.ts文件，将createGameScene中的内容全部删除，并将createGameScene方法后边的所有方法删除。

# Egret程序的入口

Egret是从一个称之为文档类的类作为入口的。当然，具体点儿，就是以这个文档类的构造函数作为入口。

文档类是可以配置的，每个Egret项目都有一个index.html文件,可以在Egret项目的根目录找到这个文件，后边我们称之为index文件。

index.html可以打开直接编辑。打开该文件，找到div标签可以看到一些配置属性，其中的data-entry-class，其值默认为Main，就是指Main.ts中所定义的类Main。当然这只是个默认值，我们可以根据自己的喜好随意修改，只要确保项目设定文件中的index.html属性值所指定的类名在项目中有其类定义即可。

# 入口函数

文档类的构造函数，即为Egret程序的入口函数，可以观察到入口函数内有一行代码

```js
this.addEventListener(egret.Event.ADDED_TO_STAGE, this.onAddToStage, this);
```

这行代码保证了onAddToStage方法执行时，文档类实例已经被添加到舞台中。并且在onAddToStage方法内，this.stage属性已经有效，其指向舞台对象。

# 绘制一个单色背景

```js
createGameScene方法

var bg:egret.Shape = new egret.Shape();

bg.graphics.beginFill( 0x336699 );
bg.graphics.drawRect( 0, 0, this.stage.stageWidth, this.stage.stageHeight );
bg.graphics.endFill();

this.addChild( bg );
```

egret.Shape对象有图形绘制功能，Shape对象中有graphic属性，用来专门负责图形绘制的工作。

第1行，在绘制前，需要定义图形的填充颜色，我们给出一个偏蓝的颜色。注意颜色值跟大部分其他OOP语言一样，是用十六进制的RGB颜色的组合来表示。用beginFill来设置填充颜色。

第2行绘制矩形。用drawRect来绘制矩形。参数部分，给出4个数字确定了一个矩形的区域，这4个参数分为两组，分别这顶区域的起点坐标和终点坐标。 注意这两组坐标，由于我们绘制的背景要刚好覆盖整个舞台区域，所以起点坐标为(0,0)，终点坐标为舞台的宽度和舞台的高度。这一对尺寸可以直接从前一节所讲到的this.stage中获得。 类似的绘制函数还有drawCircle等，可以绘制不同的简单形状。

第3行endFill用来结束绘制工作。

到这里，一张跟舞台同样大的浅蓝色矩形图形已经准备好。但，接下来我们还需要将其添加到显示结构中，才可以在运行时显示出来，就是将某个显示对象添加到某个显示容器上。 (注意，文档类实例添加到舞台是引擎默认完成的，不需要我们写代码完成该任务。)

# 调整屏幕适配模式

Egret提供了几种常规的适配模式

* showAll 不缩放
* fixedWidth 占满屏幕

fixedWidth模式下会保持原始宽高比缩放内容，缩放后应用程序内容在水平和垂直方向都填满播放器窗口，但只保持应用程序内容的原始宽度不变，高度可能会改变。

我们可以在index.html中修改适配模式，打开项目根目录下的index.html文件，找到data-scale-mode开始的代码行，将等号右边的内容修改

# 显示文字

```js
var tx:egret.TextField = new egret.TextField();
tx.text = "I'm Jack, I will use Egret create a fantasy mobile game!";
tx.size = 32;
this.addChild( tx );
tx.x = 20;
tx.y = 20;
tx.width = this.stage.stageWidth - 40;
```

# 响应用户操作

 在前一节的基础上，我们就对这段文字添加一些响应操作。比如，改变颜色：

```js
tx.touchEnabled = true;
tx.addEventListener( egret.TouchEvent.TOUCH_TAP, this.touchHandler, this );
```

注意，第一行设置touchEnabled为true，意即允许该显示对象响应Touch事件，这是Egret中特别需要注意的问题。因为所有的显示对象，默认都是不响应Touch事件的，这是基于性能考虑，因为打开对这种事件的响应，是对性能有不可忽略的影响的。 其中第二行代码新增一个方法的引用，这就是事件处理函数，我们需要事件处理函数中对用户操作做出对应的反应。 在Main类中，加入如下代码：

```js
private touchHandler( evt:egret.TouchEvent ):void{
    var tx:egret.TextField = evt.currentTarget;
    tx.textColor = 0x00ff00;
}
```

# 资源加载

Egret中所有的资源都是动态加载的。

# 资源加载清单

Egret的资源加载方式，是以一个资源加载清单为基础的。  如果资源是我们要购买的若干物品，资源加载配置就好比是购物清单。我们首先把需要购买的物品条理清晰地列在清单上，就可以按步骤有条不紊的去购买其上所列出的物品。

在Egret中，我们使用json格式作为RES资源加载配置文件的格式。

通常Egret中的资源加载配置文件位于项目目录的resource文件夹内，取名default.res.json。Egret项目在创建时已经包含该文件。

```js
{
  "resources": [
    {
      "name": "bgImage",
      "type": "image",
      "url": "assets/bg.jpg"
    },
    {
      "name": "egretIcon",
      "type": "image",
      "url": "assets/egret_icon.png"
    },
    {
      "name": "description",
      "type": "json",
      "url": "config/description.json"
    }
  ],
    "groups": [
      {
        "name": "preload",
        "keys": "bgImage,egretIcon"
      }
    ]
}
```

配置文件中的”resource”我们可以视为资源库，当前游戏使用到的资源都可以放到这里。其中以资源为单位分别列出。每一项资源单位都包含三个属性：

* name：表示这个资源的唯一标识符。注意资源比较多的项目应确定一套命名规则，避免不同资源命名之间重复或太接近而易混淆。

* type：表示资源类型。紧跟着会进一步讲解其含义及取值规则。

* url：表示当前资源文件的路径。通常我们约定配置类型的资源置于config子目录下；其他类型置于assets子目录下。

* 每个”resource”单位中的type，是Egret约定好的若干类型，最常用的有以下类型：

* image：表示各种常见的图片类型，包括PNG和JPG格式，载入后将解析为egret.Texture对象；

* text：表示文本类型，即文本文件，载入后将解析为string对象；

* json：也是一种文本类型，不过内容是json格式的，载入后将直接解析为json对象；

“groups”是预加载资源组的配置，每项是一个资源组。 每一个资源组须包含两个属性：（将若干项资源定义为一个资源组。需要时，只需加载这个资源组即可。）

* name：表示资源组的组名

* keys：表示这个资源组包含哪些资源，里面的逗号分隔的每一个字符串，都与”resource”下的资源name对应。

# 在程序中加载资源

在Main.ts的loadResource中修改RES.loadGroup的资源组名字。

很显然，loadGroup就是用来加载资源组的。

# 显示图片

```js
var batman:egret.Bitmap = new egret.Bitmap( RES.getRes("hero-01") );
batman.x = -30;
batman.y = 20;
this.addChild( batman );
```

显示所需的图片，在Egret对应的类就是Bitmap。 使用Bitmap创建一个图片时，在其构造函数中传入RES载入的资源，这里取得的是一个图片的资源，图片资源通过getRes获得的将是一个Texture对象。 这段代码用01号英雄图片资源创建了一个位图(Bitmap)显示对象，然后设定其坐标。最后添加到显示列表中。

# 显示深度控制

在某一个显示容器内的显示对象结构，可以称之为显示列表。这是由于处于同一显示容器的显示对象，是以类似列表的方式来管理的，每一个显示对象都有其索引，并且索引是从0开始的整数。显示列表索引通常称为显示深度。 我们的眼睛对着屏幕，显示列表中显示对象的索引顺序在从屏幕到眼睛方向就是从0开始，依次递增的。

例：获得当前的显示深度

```js
console.log( "display indexes:", this.getChildIndex( bg ), this.getChildIndex( batman ),  this.getChildIndex( captain ), this.getChildIndex( superman ), this.getChildIndex( hulk ) );
```

# 修改显示深度

```js
this.setChildIndex( batman, this.getChildIndex( captain ) );
```

关于显示深度的规则：

A. 某一个显示深度只能对应一个显示对象，一个显示对象也只能有一个显示深度。

B. 显示深度总是从零开始连续的，当某个深度位置的显示对象被设置为其他深度时，原来的深度会自动被紧邻的比其深度值大1位置的显示对象占据，后续深度位置的显示对象会依次往前排。

C. 某一容器内的显示列表的深度最大值是显示列表长度-1。

可以理解每个深度位置是一个房间，这些房间从0开始依次编号。每个显示对象是一个人，显示列表维护总要确保每个人在一个自己的房间，并且这些房间之间没有空的，除非这些人都从0开始排满了前面的房间。

# 交换显示深度

```js
this.swapChildren( superman, hulk );
```

# 不可逾越的显示深度最大值

```js
this.setChildIndex( captain, 20 );
```

即使设置成20，但是打印出来的最大值仍然是4

# Tween动画效果

设计某种属性（比如位置、透明度和缩放）的两个不同状态，然后在给定的时间内从一个状态平滑过渡到另外一个状态。

## 认识锚点

锚点：定位点。锚点自然地包含了两个部分：X方向锚点和Y方向锚点。anchorOffsetX和anchorOffsetY

```js
// 例：
hulk.anchorOffsetX = 30;
hulk.anchorOffsetY = 40;
hulk.x += 30;
hulk.y += 40;
```

## 设计并实现一组Tween动画

