---
title: jquery(三)事件篇
date: 2017-08-18 15:15:47
categories:
- 前端技术篇
tags:
- jQuery
comments: false
---

#### jQuery鼠标事件之click与dblclick事件
前者单击，后者双击

#### jQuery鼠标事件之mousedown与mouseup事件
#### jQuery鼠标事件之mousemove事件
移动事件

#### jQuery鼠标事件之mouseover与mouseout事件
#### jQuery鼠标事件之mouseenter与mouseleave事件
mouseenter事件只会在绑定它的元素上被调用，而不会在后代节点上被触发

#### jQuery鼠标事件之hover事件

```
(selector).hover(handlerIn, handlerOut)
```

- handlerIn(eventObject)：当鼠标指针进入元素时触发执行的事件函数
- handlerOut(eventObject)：当鼠标指针离开元素时触发执行的事件函数
#### jQuery鼠标事件之focusin事件
当一个元素，或者其内部任何一个元素获得焦点的时候，例如：input元素，用户在点击聚焦的时候，如果开发者需要捕获这个动作的时候，jQuery提供了一个focusin事件

#### jQuery鼠标事件之focusout事件
当一个元素，或者其内部任何一个元素失去焦点的时候，比如input元素，用户在点击失去焦的时候，如果开发者需要捕获这个动作，jQuery提供了一个focusout事件

#### jQuery表单事件之blur与focus事件
focus与blur事件：不支持冒泡，focusin与focusout支持冒泡

#### jQuery表单事件之change事件
元素，和元素的值都是可以发生改变的，开发者可以通过change事件去监听这些改变的动作

(1).input元素
监听value值的变化，当有改变时，失去焦点后触发change事件。对于单选按钮和复选框，当用户用鼠标做出选择时，该事件立即触发。

(2).select元素
对于下拉选择框，当用户用鼠标作出选择时，该事件立即触发

(3).textarea元素
多行文本输入框，当有改变时，失去焦点后触发change事件

#### jQuery表单事件之select事件
当 textarea 或文本类型的 input 元素中的文本被选择时，会发生 select 事件。

select事件只能用于<input>元素与<textarea>元素
#### jQuery表单事件之submit事件
(1).具体能触发submit事件的行为：
 提交提交

当某些表单元素获取焦点时，敲击Enter（回车键）

(2).这里需要特别注意：
form元素是有默认提交表单的行为，如果通过submit处理的话，需要禁止浏览器的这个默认行为
传统的方式是调用事件对象 e.preventDefault() 来处理， jQuery中可以直接在函数中最后结尾return false即可


```
$("#target").submit(function(data) { 
   return false; //阻止默认行为，提交表单
});
```

#### 13.jQuery键盘事件之keydown()与keyup()事件
#### 14.jQuery键盘事件之keypress()事件
在input元素上绑定keydown事件会发现一个问题：
每次获取的内容都是之前输入的，当前输入的获取不到

keypress()可以捕获当前的值，但是中文不可以，e.target.value返回之前所有的输入字符，这个时候中文 就会被提取并打印出来！

#### 15.on()的多事件绑定
(1).基本用法：.on( events ,[ selector ] ,[ data ] )

```
$("#elem").click(function(){})  //快捷方式
$("#elem").on('click',function(){}) //on方式
最大的不同点就是on是可以自定义事件名
```

(2).多个事件绑定同一个函数

```
$("#elem").on("mouseover mouseout",function(){ });
```

(3).多个事件绑定不同函数

```
$("#elem").on({
    mouseover:function(){},  
    mouseout:function(){}
});
```

(4).将数据传递到处理程序

```
function greet( event ) {
  alert( "Hello " + event.data.name ); //Hello 慕课网
}
$( "button" ).on( "click", {
  name: "慕课网"
}, greet );
```

可以通过第二参数（对象），当一个事件被触发时，要传递给事件处理函数的

#### 16.on()的高级用法
事件绑定在最上层div元素上，当用户触发在a元素上，事件将往上冒泡，一直会冒泡在div元素上。如果提供了第二参数，那么事件在往上冒泡的过程中遇到了选择器匹配的元素，将会触发事件回调函数


```
<div class="left">
    <p class="aaron">
        <a>目标节点</a> //点击在这个元素上
    </p>
</div>

$("div").on("click","a",fn)
```

#### 17.卸载事件off()方法
(1).绑定2个事件

```
$("elem").on("mousedown mouseup",fn)
```

(2).删除一个事件

```
$("elem").off("mousedown")
```

(3).删除所有事件

```
$("elem").off("mousedown mouseup")
```

快捷方式删除所有事件，这里不需要传递事件名了，节点上绑定的所有事件讲全部销毁


```
$("elem").off()
```

#### 18.jQuery事件对象的作用

```
<ul>
    <li class="even1"></li>
    <li class="even2"></li>
    <li class="even2"></li>
    .........
</ul>
```

ul有N个子元素li(这里只写了3个)，如果我要响应每一个li的事件，那么常规的方法就是需要给所有的li都单独绑定一个事件监听，这样写法很符合逻辑，但是同时有显得繁琐

因为li都有一个共同的父元素，而且所有的事件都是一致的，这里我们可以采用要一个技巧来处理，也是常说的”事件委托”

事件没直接和li元素发生关系，而且绑定父元素了。由于浏览器有事件冒泡的这个特性，我们可以在触发li的时候把这个事件往上冒泡到ul上，因为ul上绑定事件响应所以就能够触发这个动作了。唯一的问题怎么才知道触发的li元素是哪个一个？

这里就引出了事件对象了

> 事件对象是用来记录一些事件发生时的相关信息的对象。事件对象只有事件发生时才会产生，并且只能是事件处理函数内部访问，在所有事件处理函数运行结束后，事件对象就被销毁

event.target代表当前触发事件的元素，可以通过当前元素对象的一系列属性来判断是不是我们想要的元素

#### 19.jQuery事件对象的属性和方法
事件对象的属于与方法有很多，但是我们经常用的只有那么几个，这里我主要说下作用与区别

(1).event.type：获取事件的类型
触发元素的事件类型


```
$("a").click(function(event) {
  alert(event.type); // "click"事件
});
```

(2).event.pageX 和 event.pageY：获取鼠标当前相对于页面的坐标

通过这2个属性，可以确定元素在当前页面的坐标值，鼠标相对于文档的左边缘的位置（左边）与 （顶边）的距离，简单来说是从页面左上角开始,即是以页面为参考点,不随滑动条移动而变化

(3).event.preventDefault()

方法：阻止默认行为
这个用的特别多，在执行这个方法后，如果点击一个链接（a标签），浏览器不会跳转到新的 URL 去了。我们可以用 event.isDefaultPrevented() 来确定这个方法是否(在那个事件对象上)被调用过了

(4).event.stopPropagation() 

方法：阻止事件冒泡
事件是可以冒泡的，为防止事件冒泡到DOM树上，也就是不触发的任何前辈元素上的事件处理函数

(5).event.which：获取在鼠标单击时，单击的是鼠标的哪个键

event.which 将 event.keyCode 和 event.charCode 标准化了。event.which也将正常化的按钮按下(mousedown 和 mouseupevents)，左键报告1，中间键报告2，右键报告3

(6).event.currentTarget : 

在事件冒泡过程中的当前DOM元素
冒泡前的当前触发事件的DOM对象, 等同于this.

(7).this和event.target的区别：

js中事件是会冒泡的，所以this是可以变化的，但event.target不会变化，它永远是直接接受事件的目标DOM元素；

(8).this和event.target都是dom对象

如果要使用jquey中的方法可以将他们转换为jquery对象。比如this和$(this)的使用、event.target和$(event.target)的使用；

#### 20.jQuery自定义事件之trigger事件
trigger() 方法触发被选元素的指定事件类型。


```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("input").select(function(){
    $("input").after("文本被选中！");
  });
  $("button").click(function(){
    $("input").trigger("select");
  });
});
</script>
</head>
<body>
<input type="text" name="FirstName" value="Hello World" />
<br />
<button>激活 input 域的 select 事件</button>
</body>
</html>
```

#### 21.jQuery自定义事件之triggerHandler事件
triggerHandler() 方法触发被选元素的指定事件类型。但不会执行浏览器默认动作，也不会产生事件冒泡。

triggerHandler() 方法与 trigger() 方法类似。不同的是它不会触发事件（比如表单提交）的默认行为，而且只影响第一个匹配元素。


```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("input").select(function(){
    $("input").after("发生 Input select 事件！");
  });
  $("button").click(function(){
    $("input").triggerHandler("select");
  });
});
</script>
</head>
<body>
<input type="text" name="FirstName" value="Hello World" />
<br />
<button>激活 input 域的 select 事件</button>
<p>请注意，与 trigger() 方法不同，triggerHandler() 方法不会引起所发生事件的默认行为（文本不会被选中）。</p>
</body>
</html>
```
