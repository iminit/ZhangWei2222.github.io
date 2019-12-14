---
title: jquery(一)样式篇
date: 2017-08-18 15:15:47
categories:
- JavaScript
tags:
- jQuery
comments: false
---

### 1.$(document).ready
作用是等页面的文档（document）中的节点都加载完毕后，再执行后续的代码，因为我们在执行代码的时候，可能会依赖页面的某一个元素，我们要确保这个元素真正的的被加载完毕后才能正确的使用。

### 2.jQuery对象与DOM对象
通过一个简单的例子，简单区分下jQuery对象与DOM对象：
我们要获取页面上这个id为imooc的p元素，然后给这个文本节点增加一段文字：“您好！通过慕课网学习jQuery才是最佳的途径”，并且让文字颜色变成红色。

普通处理，通过标准JavaScript处理：

v
```
ar p = document.getElementById('imooc');
p.innerHTML = '您好！通过慕课网学习jQuery才是最佳的途径';
p.style.color = 'red';
```

通过原生DOM模型提供的document.getElementById(“imooc”) 方法获取的DOM元素就是一个DOM对象，再通过innerHTML与style属性处理文本与颜色。

jQuery的处理：


```
var $p = $('#imooc');
$p.html('您好！通过慕课网学习jQuery才是最佳的途径').css('color','red');
```

通过$(‘#imooc’)方法会得到一个$p的jQuery对象，$p是一个类数组对象。这个对象里面包含了DOM对象的信息，然后封装了很多操作方法，调用自己的方法html与css，得到的效果与标准的JavaScript处理结果是一致的。

### 3.jQuery对象转化成DOM对象
在很多场景中，我们需要jQuery与DOM能够相互的转换，它们都是可以操作的DOM元素，jQuery是一个类数组对象，而DOM对象就是一个单独的DOM元素。

如何把jQuery对象转成DOM对象？

#### (1).利用数组下标的方式读取到jQuery中的DOM对象
HTML代码

元素一

元素二

元素三
JavaScript代码


```
var $div = $('div') //jQuery对象
var div = $div[0] //转化成DOM对象
div.style.color = 'red' //操作dom对象的属性
```

用jQuery找到所有的div元素（3个），因为jQuery对象也是一个数组结构，可以通过数组下标索引找到第一个div元素，通过返回的div对象，调用它的style属性修改第一个div元素的颜色。这里需要注意的一点是，数组的索引是从0开始的，也就是第一个元素下标是0

#### (2).通过jQuery自带的get()方法
jQuery对象自身提供一个.get() 方法允许我们直接访问jQuery对象中相关的DOM节点，get方法中提供一个元素的索引：


```
var $div = $('div') //jQuery对象
var div = $div.get(0) //通过get方法，转化成DOM对象
div.style.color = 'red' //操作dom对象的属性
```

其实我们翻开源码，看看就知道了，get方法就是利用的第一种方式处理的，只是包装成一个get让开发者更直接方便的使用。

### 4.DOM对象转化成jQuery对象
相比较jQuery转化成DOM，开发中更多的情况是把一个dom对象加工成jQuery对象。$(参数)是一个多功能的方法，通过传递不同的参数而产生不同的作用。

如果传递给$(DOM)函数的参数是一个DOM对象，jQuery方法会把这个DOM对象给包装成一个新的jQuery对象

通过$(dom)方法将普通的dom对象加工成jQuery对象之后，我们就可以调用jQuery的方法了

HTML代码

元素一

元素二

元素三
JavaScript代码


```
var div = document.getElementsByTagName('div'); //dom对象
var $div = $(div); //jQuery对象
var $first = $div.first(); //找到第一个div元素
$first.css('color', 'red'); //给第一个元素设置颜色
```

通过getElementsByTagName获取到所有div节点的元素，结果是一个dom合集对象，不过这个对象是一个数组合集(3个div元素)。通过$(div)方法转化成jQuery对象，通过调用jQuery对象中的first与css方法查找第一个元素并且改变其颜色。

### 5.jQuery选择器之id选择器

```
$( "#id" )
```

id是唯一的，每个id值在一个页面中只能使用一次。如果多个元素分配了相同的id，将只匹配该id选择集合的第一个DOM元素。但这种行为不应该发生;有超过一个元素的页面使用相同的id是无效的

### 6.jQuery选择器之类选择器

```
$( ".class" )
```

### 7.jQuery选择器之元素选择器

```
$( "element" )
```

### 8.jQuery选择器之全选择器（*选择器）

```
$( "*" )
```

### 9.jQuery选择器之层级选择器


1. 层级选择器都有一个参考节点
1. 后代选择器包含子选择器的选择的内容
1. 一般兄弟选择器包含相邻兄弟选择的内容
1. 相邻兄弟选择器和一般兄弟选择器所选择到的元素，必须在同一个父元素下
### 10.jQuery选择器之基本筛选选择器


1. :eq(), :lt(), :gt(), :even, :odd 用来筛选他们前面的匹配表达式的集合元素，根据之前匹配的元素在进一步筛选，注意jQuery合集都是从0开始索引
1. gt是一个段落筛选，从指定索引的下一个开始，gt(1) 实际从2开始
### 11.jQuery选择器之内容筛选选择器


1. :contains与:has都有查找的意思，但是contains查找包含“指定文本”的元素，has查找包含“指定元素”的元素
1. 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
1. :parent与:empty是相反的，两者所涉及的子元素，包括文本节点
### 12.jQuery选择器之可见性筛选选择器


元素可以被认为是隐藏的几个情况：

1. 他们的CSS display值是none。
1. 他们是type=”hidden”的表单元素。
1. 它们的宽度和高度都显式设置为0。
1. 一个祖先元素是隐藏的，因此该元素是不会在页面上显示。
### 13.jQuery选择器之属性筛选选择器


在这么多属性选择器中[attr=”value”]和[attr*=”value”]是最实用的


```
[attr="value"]能帮我们定位不同类型的元素，特别是表单form元素的操作，比如说input[type="text"],input[type="checkbox"]等
[attr*="value"]能在网站中帮助我们匹配不同类型的文件
```

### 14.jQuery选择器之子元素筛选选择器
子元素筛选选择器不常使用，其筛选规则比起其它的选择器稍微要复杂点



1. :first只匹配一个单独的元素，但是:first-child选择器可以匹配多个：即为每个父级元素匹配第一个子元素。这相当于:nth-child(1)
1. :last 只匹配一个单独的元素， :last-child 选择器可以匹配多个元素：即，为每个父级元素匹配最后一个子元素
1. 如果子元素只有一个的话，:first-child与:last-child是同一个
1. :only-child匹配某个元素是父元素中唯一的子元素，就是说当前子元素是父元素中唯一的元素，则匹配
1. jQuery实现:nth-child(n)是严格来自CSS规范，所以n值是“索引”，也就是说，从1开始计数，:nth-child(index)从1开始的，而eq(index)是从0开始的
1. nth-child(n) 与 :nth-last-child(n) 的区别前者是从前往后计算，后者从后往前计算
### 15.jQuery选择器之表单元素选择器


除了input筛选选择器，几乎每个表单类别筛选器都对应一个input元素的type值。大部分表单类别筛选器可以使用属性筛选器替换。比如 $(‘:password’) == $(‘[type=password]’)

### 16.jQuery选择器之表单对象属性筛选选择器


1. 选择器适用于复选框和单选框，对于下拉框元素, 使用 :selected 选择器
1. 在某些浏览器中，选择器:checked可能会错误选取到元素，所以保险起见换用选择器input:checked，确保只会选取元素
### 17.jQuery选择器之特殊选择器this

```
HTML
p.addEventListener('click',function(){
    //this === p
    //以下两者的修改都是等价的
    this.style.color = "red";
    p.style.color = "red";
},false);

 this.style.color = "red"
$('p').click(function(){
    //把p元素转化成jQuery的对象
    var $this= $(this) 
    $this.css('color','red')
})
```

this，表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
$(this),代表的上下文对象是一个jquery的上下文对象，可以调用jQuery的方法和属性值。

### 18.jQuery的属性与样式之.attr()与.removeAttr()
#### (1).attr()有4个表达式
1. attr(传入属性名)：获取属性的值
1. attr(属性名, 属性值)：设置属性的值
1. attr(属性名,函数值)：设置属性的函数值
1. attr(attributes)：给指定元素设置多个属性值，即：{属性名一: “属性值一” , 属性名二: “属性值二” , … … }
#### (2).removeAttr()删除方法
.removeAttr( attributeName ) : 为匹配的元素集合中的每个元素中移除一个属性（attribute）

#### (3).注意的问题
dom中有个概念的区分：Attribute和Property翻译出来都是“属性”，《js高级程序设计》书中翻译为“特性”和“属性”。简单理解，Attribute就是dom节点自带的属性

> 获取Attribute就需要用attr，获取Property就需要用prop

### 19.jQuery的属性与样式之html()及.text()
读取、修改元素的html结构或者元素的文本内容是常见的DOM操作，jQuery针对这样的处理提供了2个便捷的方法.html()与.text()

#### (1).html()方法
获取集合中第一个匹配元素的HTML内容 或 设置每一个匹配元素的html内容，具体有3种用法：

1. .html() 不传入值，就是获取集合中第一个匹配元素的HTML内容
1. .html( htmlString ) 设置每一个匹配元素的html内容
1. .html( function(index, oldhtml) ) 用来返回设置HTML内容的一个函数
#### (2).注意事项：
.html()方法内部使用的是DOM的innerHTML属性来处理的，所以在设置与获取上需要注意的一个最重要的问题，这个操作是针对整个HTML内容（不仅仅只是文本内容）

#### (3).text()方法
得到匹配元素集合中每个元素的文本内容结合，包括他们的后代，或设置匹配元素集合中每个元素的文本内容为指定的文本内容。，具体有3种用法：

1. .text() 得到匹配元素集合中每个元素的合并文本，包括他们的后代
1. .text( textString ) 用于设置匹配元素内容的文本
1. .text( function(index, text) ) 用来返回设置文本内容的一个函数
#### (4).注意事项：
.text()结果返回一个字符串，包含所有匹配元素的合并文本

#### (5).html与.text的异同:
1. .html与.text的方法操作是一样，只是在具体针对处理对象不同
1. .html处理的是元素内容，.text处理的是文本内容
1. .html只能使用在HTML文档中，.text 在XML 和 HTML 文档中都能使用
1. 如果处理的对象只有一个子文本节点，那么html处理的结果与text是一样的
1. 火狐不支持innerText属性，用了类似的textContent属性，.text()方法综合了2个属性的支持，所以可以兼容所有浏览器
### 20.jQuery的属性与样式之.val()
jQuery中有一个.val()方法主要是用于处理表单元素的值，比如 input, select 和 textarea。

#### (1).val()方法
1. .val()无参数，获取匹配的元素集合中第一个元素的当前值
1. .val( value )，设置匹配的元素集合中每个元素的值
1. .val( function ) ，一个用来返回设置值的函数

#### (2)注意事项：
1. 通过.val()处理select元素， 当没有选择项被选中，它返回null
1. .val()方法多用来设置表单的字段的值
1. 如果select元素有multiple（多选）属性，并且至少一个选择项被选中， .val()方法返回一个数组，这个数组包含每个选中选择项的值

#### (3).html(),.text()和.val()的差异总结：
1. .html(),.text(),.val()三种方法都是用来读取选定元素的内容；只不过.html()是用来读取元素的html内容（包括html标签），.text()用来读取元素的纯文本内容，包括其后代元素，.val()是用来读取表单元素的”value”值。其中.html()和.text()方法不能使用在表单元素上,而.val()只能使用在表单元素上；另外.html()方法使用在多个元素上时，只读取第一个元素；.val()方法和.html()相同，如果其应用在多个元素上时，只能读取第一个表单元素的”value”值，但是.text()和他们不一样，如果.text()应用在多个元素上时，将会读取所有选中元素的文本内容。
1. .html(htmlString),.text(textString)和.val(value)三种方法都是用来替换选中元素的内容，如果三个方法同时运用在多个元素上时，那么将会替换所有选中元素的内容。
1. .html(),.text(),.val()都可以使用回调函数的返回值来动态的改变多个元素的内容。

### 21.jQuery的属性与样式之增加样式.addClass()
通过动态改变类名（class），可以让其修改元素呈现出不同的效果。在HTML结构中里，多个class以空格分隔，当一个节点（或称为一个标签）含有多个class时，DOM元素响应的className属性获取的不是class名称的数组，而是一个含有空格的字符串，这就使得多class操作变得很麻烦。同样的jQuery开发者也考虑到这种情况，增加了一个.addClass()方法，用于动态增加class类名

#### (1).addClass( className )方法
1. .addClass( className ) : 为每个匹配元素所要增加的一个或多个样式名
1. .addClass( function(index, currentClass) ) : 这个函数返回一个或更多用空格隔开的要增加的样式名
#### (2).注意事项：
.addClass()方法不会替换一个样式类名。它只是简单的添加一个样式类名到元素上
#### (3).
简单的描述下：在p元素增加一个newClass的样式


```
<p class="orgClass">
$("p").addClass("newClass")
```

那么p元素的class实际上是 class=”orgClass newClass”样式只会在原本的类上继续增加，通过空格分隔

### 22.jQuery的属性与样式之删除样式.removeClass()
jQuery通过.addClass()方法可以很便捷的增加样式。如果需要样式之间的切换，同样jQuery提供了一个很方便的.removeClass()，它的作用是从匹配的元素中删除全部或者指定的class

#### (1).removeClass( )方法
- .removeClass( [className ] )：每个匹配元素移除的一个或多个用空格隔开的样式名
- .removeClass( function(index, class) ) ： 一个函数，返回一个或多个将要被移除的样式名
#### (2).注意事项
如果一个样式类名作为一个参数,只有这样式类会被从匹配的元素集合中删除 。 如果没有样式名作为参数，那么所有的样式类将被移除

### 23.jQuery的属性与样式之切换样式.toggleClass()
在做某些效果的时候，可能会针对同一节点的某一个样式不断的切换，也就是addClass与removeClass的互斥切换，比如隔行换色效果

jQuery提供一个toggleClass方法用于简化这种互斥的逻辑，通过toggleClass方法动态添加删除Class，一次执行相当于addClass，再次执行相当于removeClass

#### (1).toggleClass( )方法：
在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类

1. .toggleClass( className )：在匹配的元素集合中的每个元素上用来切换的一个或多个（用空格隔开）样式类名
1. .toggleClass( className, switch )：一个布尔值，用于判断样式是否应该被添加或移除
1. .toggleClass( [switch ] )：一个用来判断样式类添加还是移除的 布尔值
1. .toggleClass( function(index, class, switch) [, switch ] )：用来返回在匹配的元素集合中的每个元素上用来切换的样式类名的一个函数。接收元素的索引位置和元素旧的样式类作为参数
#### (2).注意事项：
1. toggleClass是一个互斥的逻辑，也就是通过判断对应的元素上是否存在指定的Class名，如果有就删除，如果没有就增加
1. toggleClass会保留原有的Class名后新增，通过空格隔开

### 24.jQuery的属性与样式之样式操作.css()
通过JavaScript获取dom元素上的style属性，我们可以动态的给元素赋予样式属性。在jQuery中我们要动态的修改style属性我们只要使用css()方法就可以实现了

.css() 方法：获取元素样式属性的计算值或者设置元素的CSS属性

#### (1).获取：
- .css( propertyName ) ：获取匹配元素集合中的第一个元素的样式属性的计算值
- .css( propertyNames )：传递一组数组，返回一个对象结果

#### (2).设置：
.css(propertyName, value )：设置CSS


```
css(“background-color”,”red”)
css(“backgroundColor”,”yellow”)
```


.css( propertyName, function )：可以传入一个回调函数，返回取到对应的值进行处理

.css( properties )：可以传一个对象，同时设置多个样式


```
css({
    'font-size'        :"15px",
    "background-color" :"#40E0D0",
    "border"           :"1px solid red"
})
```

#### (3).注意事项：
1. 浏览器属性获取方式不同，在获取某些值的时候都jQuery采用统一的处理，比如颜色采用RBG，尺寸采用px
1. .css()方法支持驼峰写法与大小写混搭的写法，内部做了容错的处理
1. 当一个数只被作为值（value）的时候， jQuery会将其转换为一个字符串，并添在字符串的结尾处添加px，例如 .css(“width”,50}) 与 .css(“width”,”50px”})一样

### 25.jQuery的属性与样式之.css()与.addClass()设置样式的区别

#### (1).可维护性：
.addClass()的本质是通过定义个class类的样式规则，给元素添加一个或多个类。css方法是通过JavaScript大量代码进行改变元素的样式

通过.addClass()我们可以批量的给相同的元素设置统一规则，变动起来比较方便，可以统一修改删除。如果通过.css()方法就需要指定每一个元素是一一的修改，日后维护也要一一的修改，比较麻烦

#### (2).灵活性：
通过.css()方式可以很容易动态的去改变一个样式的属性，不需要在去繁琐的定义个class类的规则。一般来说在不确定开始布局规则，通过动态生成的HTML代码结构中，都是通过.css()方法处理的

#### (3).样式值：
.addClass()本质只是针对class的类的增加删除，不能获取到指定样式的属性的值，.css()可以获取到指定的样式值。

#### (4).样式的优先级
通过.css方法设置的样式属性优先级要高于.addClass方法

#### (5).总结
.addClass与.css方法各有利弊，一般是静态的结构，都确定了布局的规则，可以用addClass的方法，增加统一的类规则

如果是动态的HTML结构，在不确定规则，或者经常变化的情况下，一般多考虑.css()方式

### 26.jQuery的属性与样式之元素的数据存储
.data( key, value ) //实例接口,存数据
.data( key ) //实例接口,存数据

删除

jQuery.removeData( element [, name ] )
.removeData( [name ] )

