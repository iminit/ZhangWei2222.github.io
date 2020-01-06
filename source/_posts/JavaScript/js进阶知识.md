---
title: js进阶知识
date: 2017-08-18 15:15:47
categories:
- JavaScript
tags:
comments: false
---

### 一、变量名
#### 1.命名
1.必须以字母、下划线或美元符号开头，后面可以跟字母、下划线、美元符号和数字。

2.变量名区分大小写，如:A与a是两个不同变量。

3.不允许使用JavaScript关键字和保留字做变量名。

<!-- more -->

#### 2.声明

```
var mynum ; //声明一个变量mynum
var num1,mun2 ; //声明一个变量num1
```

#### 3.变量赋值
= 是赋值，==是等于号


```
var num1 = 123;       // 123是数值
var num2 = "一二三";    //"一二三"是字符串
var num3=true;    //布尔值true（真），false(假)
```

### 二、数组
#### 1.二维数组

```
var myarr=new Array();  //先声明一维 
for(var i=0;i<2;i++){   //一维长度为2
   myarr[i]=new Array();  //再声明二维 
   for(var j=0;j<3;j++){   //二维长度为3
   myarr[i][j]=i+j;   // 赋值，每个数组元素的值为i+j
   }
 }
var Myarr = [[0 , 1 , 2 ],[1 , 2 , 3]]
```

### 三、鼠标事件


#### 1.onfocus（获得焦点事件）
当一个文本框获得焦点时，它里面的文本就像“好123”网站上的百度搜索输入框那样全部被自动选中，这样的操作可以利用onfocus来实现。

以下的文本框，当鼠标指针移过去时，里面的文字全部被选中：

请输入网址

这是怎么做的呢？看以下代码及解释：


```
<input type="text"name="url" value="http://www.gxblk.com" size="30"onmousemove="this.focus();this.select();">
```

代码里，input标签内嵌入了onmousemove（鼠标指针经过）事件的JS语句，其等号后面的this.focus()意为其自身获得焦点；获得焦点的标志是该文本框内将出现输入光标，但要让其内的文字全部被选中，我们还得用上this.select()语句，它的意思就是选中全部文本框里的文字。

#### 2.onblur（失去焦点事件）
我们经常会检测文本框是否已经被正确输入，检测工作通常在用户点击了提交按钮之后进行，事实上，利用控件失去焦点的时候，我们就可以实时进行这个检测工作，这样的话，onblur事件就派上用场了。

以下例子有四个文本框，如果还没有任何单击它们当中的任意一个的操作，那么什么事情也不会发生，但是，当你单击了其中的任何一个使其拥有了焦点（输入光标在里面），如果什么都没有输入并且单击了别的地方令其失去焦点，就会弹出一个警告，试试看

姓名

性别

年龄

住址

以下是代码和解释：

表单代码


```
<form name="blur_test"> 
<p>姓名 <input type="text" name="name"value="" size="30"onblur="chkvalue(this)"><br> 
性别 <inputtype="text" name="sex" value=""size="30" onblur="chkvalue(this)"><br> 
年龄 <inputtype="text" name="age" value=""size="30" onblur="chkvalue(this)"><br> 
住址 <inputtype="text" name="addr" value=""size="30" onblur="chkvalue(this)"></p> 
</form>

<scriptlanguage="javascript"> 
function chkvalue(txt) { 
if(txt.value=="") alert("文本框里必须填写内容!"); 
} 
</script>
```

> 表单代码里，每一个方框框的代码都嵌入一个onblur JS语句，它们都调用后面的JS代码中的自定义函数chkvalue(this)，意思是，当文本框失去焦点时就调用chkvalue()函数；这chkvalue()函数检测文本框是否为空，如果是就弹出警告窗口。该函数有一个参数（txt），对应于前面文本框调用该函数的参数（this）即自身。

#### 3.onselect事件
选中事件，当文本框或者文本域中的文字被选中时，触发onselect事件，同时调用的程序就会被执行。

#### 4.文本框内容改变事件（onchange）
通过改变文本框的内容来触发onchange事件，同时执行被调用的程序。

#### 5.加载事件（onload）
事件会在页面加载完成后，立即发生，同时执行被调用的程序。

注意：

1、加载页面时，触发onload事件，事件写在标签内。

2、此节的加载页面，可理解为打开一个新页面时。

如下代码,当加载一个新页面时，弹出对话框“加载中，请稍等…”。

> 支持的标签：, , , , , 提交提交, , ,，写在内监听页面加载，正如写在监听图像加载。

#### 6.卸载事件（onunload）
当用户退出页面时（页面关闭、页面刷新等），触发onUnload事件，同时执行被调用的程序。

**注意：不同浏览器对onunload事件支持不同。
**

如下代码,当退出页面时，弹出对话框“您确定离开该网页吗？”。

### 四、 JavaScript内置对象
#### 1.Date 日期对象
定义一个时间对象 :


```
var Udate=new Date();
```

使 Udate 成为日期对象，并且已有初始值：当前时间(当前电脑系统时间)。

访问方法语法：“<日期对象>.<方法>”



##### (1).返回星期方法
getDay() 返回星期，返回的是0-6的数字，0 表示星期天。如果要返回相对应“星期”，通过数组完成，代码如下:


```
<script type="text/javascript">
  var mydate=new Date();//定义日期对象
  var weekday=["星期日","星期一","星期二","星期三","星期四","星期五","星期六"];
//定义数组对象,给每个数组项赋值
  var mynum=mydate.getDay();//返回值存储在变量mynum中
  document.write(mydate.getDay());//输出getDay()获取值
  document.write("今天是："+ weekday[mynum]);//输出星期几
</script>
```

##### (2).返回/设置时间方法
get/setTime() 返回/设置时间，单位毫秒数，计算从 1970 年 1 月 1 日零时到日期对象所指的日期的毫秒数。

#### 2.String字符串对象
##### (1).toUpperCase()，toLowerCase()
使用 String 对象的 toUpperCase() 方法来将字符串小写字母转换为大写,另一个反之

##### (2).charAt()
stringObject.charAt(index)
可返回指定位置的字符。返回的字符是长度为 1 的字符串。

##### (3).字符串分割split()
split() 方法将字符串分割为字符串数组，并返回此数组。


```
stringObject.split(separator,limit)
```

注意：如果把空字符串 (“”) 用作 separator，那么 stringObject 中的每个字符之间都会被分割。


```
var mystr = "www.imooc.com";
document.write(mystr.split(".")+"<br>");
document.write(mystr.split(".", 2)+"<br>");

www,imooc,com
www,imooc

document.write(mystr.split("")+"<br>");
document.write(mystr.split("", 5));

w,w,w,.,i,m,o,o,c,.,c,o,m
w,w,w,.,i
```

##### (4).提取字符串substring()
substring() 方法用于提取字符串中介于两个指定下标之间的字符


```
stringObject.substring(startPos,stopPos)
```

注意：

1. 返回的内容是从 start开始(包含start位置的字符)到 stop-1 处的所有字符，其长度为 stop 减start。
1. 如果参数 start 与 stop 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。
1. 如果 start 比 stop 大，那么该方法在提取子串之前会先交换这两个参数。
##### (5).提取指定数目的字符substr()
substr() 方法从字符串中提取从 startPos位置开始的指定数目的字符串。


```
stringObject.substr(startPos,length)
```

注意：

如果参数startPos是负数，从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。
如果startPos为负数且绝对值大于字符串长度，startPos为0。


```
string.substring( 起点 ， 终点 )

string.substr( 起点 ，长度 )
```


#### 3.Math对象
Math 对象是一个固有的对象，无需创建它，直接把 Math 作为对象使用就可以调用其所有属性和方法。这是它与Date,String对象的区别。

Math 对象属性



Math 对象方法




```
<script type="text/javascript">
  var mypi=Math.PI; 
  var myabs=Math.abs(-15);
  document.write(mypi);
  document.write(myabs);
</script>

3.141592653589793
15
```

##### (1).向上取整ceil()

```
Math.ceil(x)
```

ceil() 方法可对一个数进行向上取整。x必须是一个数值

注意：它返回的是大于或等于x，并且与x最接近的整数。

##### (2).向下取整floor()

```
Math.floor(x)
```

floor() 方法可对一个数进行向下取整。

注意：返回的是小于或等于x，并且与 x 最接近的整数。

##### (3).四舍五入round()

```
Math.round(x)
```

round() 方法可把一个数字四舍五入为最接近的整数。

注意：

1. 返回与 x 最接近的整数。
1. 对于 0.5，该方法将进行上舍入。(5.5 将舍入为 6)
1. 如果 x 与两侧整数同等接近，则结果接近 +∞方向的数字值 。(如 -5.5 将舍入为 -5; -5.52 将舍入为 -6)

##### (4).随机数 random()

```
Math.random();
```

random() 方法可返回介于 0 ~ 1（大于或等于 0 但小于 1 )之间的一个随机数。

注意：返回一个大于或等于 0 但小于 1 的符号为正的数字值。

#### 4.Array 数组对象


##### (1).数组连接concat()
concat() 方法用于连接两个或多个数组。此方法返回一个新数组，不改变原来的数组。


```
<script type="text/javascript">
  var mya = new Array(3);
  mya[0] = "1";
  mya[1] = "2";
  mya[2] = "3";
  document.write(mya.concat(4,5)+"<br>");
  document.write(mya); 
</script>

1,2,3,4,5
1,2,3
```

##### (2).指定分隔符连接数组元素join()
join()方法用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的。不改变原来数组

##### (3).颠倒数组元素顺序reverse()
该方法会改变原来的数组，而不会创建新的数组。

##### (4).选定元素slice()

```
arrayObject.slice(start,end)
```

注意：

1. 可使用负值从数组的尾部选取元素。
1. 如果 end 未被规定，那么 slice() 方法会选取从 start 到数组结尾的所有元素。
1. String.slice() 与 Array.slice() 相似。
1. 返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素。
1. 该方法并不会修改数组，而是返回一个子数组

##### (5).数组排序sort()

```
arrayObject.sort(方法函数)
```

注意

1.如果不指定<方法函数>，则按unicode码顺序排列。

2.如果指定<方法函数>，则按<方法函数>所指定的排序方法排序。

排序函数：


```
<script type="text/javascript">
  function sortNum(a,b) {
  return a - b;
 //升序，如降序，把“a - b”该成“b - a”
}
 var myarr = new Array("80","16","50","6","100","1");
  document.write(myarr + "<br>");
  document.write(myarr.sort(sortNum));
</script>


80,16,50,6,100,1
1,6,16,50,80,100
```

### 五、浏览器对象
window对象是BOM的核心，window对象指当前的浏览器窗口。

window对象方法:



#### 1.计时器
在JavaScript中，我们可以在设定的时间间隔之后来执行代码，而不是在函数被调用后立即执行。
计时器类型：

一次性计时器：仅在指定的延迟时间之后触发一次。

间隔性触发计时器：每隔一定的时间间隔就触发一次。

计时器方法：



##### (1).setInterval()
在执行时,从载入页面后每隔指定的时间执行代码。


```
setInterval(代码,交互时间);
```

参数说明：

1. 代码：要调用的函数或要执行的代码串。
1. 交互时间：周期性执行或调用表达式之间的时间间隔，以毫秒计（1s=1000ms）。

返回值:

一个可以传递给 clearInterval() 从而取消对”代码”的周期性执行的值。


```
调用函数格式(假设有一个clock()函数):
setInterval("clock()",1000)
或
setInterval(clock,1000)
```

我们设置一个计时器，每隔100毫秒调用clock()函数，并将时间显示出来，代码如下:


```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>计时器</title>
<script type="text/javascript">
  var int=setInterval(clock, 100)
  function clock(){
    var time=new Date();
    document.getElementById("clock").value = time;
  }
</script>
</head>
<body>
  <form>
    <input type="text" id="clock" size="50"  />
  </form>
</body>
</html>
```

##### (2).取消计时器clearInterval()
clearInterval() 方法可取消由 setInterval() 设置的交互时间。


```
clearInterval(id_of_setInterval)
```

参数说明:

id_of_setInterval：由 setInterval() 返回的 ID 值。

每隔 100 毫秒调用 clock() 函数,并显示时间。当点击按钮时，停止时间,代码如下:


```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>计时器</title>
<script type="text/javascript">
   function clock(){
      var time=new Date();                     
      document.getElementById("clock").value = time;
   }
// 每隔100毫秒调用clock函数，并将返回值赋值给i
     var i=setInterval("clock()",100);
</script>
</head>
<body>
  <form>
    <input type="text" id="clock" size="50"  />
    <input type="button" value="Stop" onclick="clearInterval(i)"  />
  </form>
</body>
</html>
```

##### (3).计时器setTimeout()
setTimeout()计时器，在载入后延迟指定时间后,去执行一次表达式,仅执行一次。


```
setTimeout(代码,延迟时间);
```

参数说明：

1. 要调用的函数或要执行的代码串。
1. 延时时间：在执行代码前需等待的时间，以毫秒为单位（1s=1000ms)。

要创建一个运行于无穷循环中的计数器，我们需要编写一个函数来调用其自身。在下面的代码，当按钮被点击后，输入域便从0开始计数。


```
<!DOCTYPE HTML>
<html>
<head>
<script type="text/javascript">
var num=0;
function numCount(){
 document.getElementById('txt').value=num;
 num=num+1;
 setTimeout("numCount()",1000);
 }
</script>
</head>
<body>
<form>
<input type="text" id="txt" />
<input type="button" value="Start" onClick="numCount()" />
</form>
</body>
</html>
```

##### (4).取消计时器clearTimeout()
setTimeout()和clearTimeout()一起使用，停止计时器。


```
clearTimeout(id_of_setTimeout)
<!DOCTYPE HTML>
<html>
<head>
<script type="text/javascript">
  var num=0,i;
  function timedCount(){
    document.getElementById('txt').value=num;
    num=num+1;
    i=setTimeout(timedCount,1000);
  }
    setTimeout(timedCount,1000);
  function stopCount(){
    clearTimeout(i);
  }
</script>
</head>
<body>
  <form>
    <input type="text" id="txt">
    <input type="button" value="Stop" onClick="stopCount()">
  </form>
</body>
</html>
```

#### 2.History对象
history对象记录了用户曾经浏览过的页面(URL)，并可以实现浏览器前进与后退相似导航的功能。

注意:

从窗口被打开的那一刻开始记录，每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。

window.history.[属性|方法]（window可以省略。）

History 对象属性



History 对象方法



##### (1).返回前一个浏览的页面
back()方法，加载 history 列表中的前一个 URL。


```
window.history.back();
```

比如，返回前一个浏览的页面，代码如下：

```
window.history.back();
```

注意：等同于点击浏览器的倒退按钮。

back()相当于go(-1),代码如下:


```
window.history.go(-1);
```

##### (2).返回下一个浏览的页面
forward()方法，加载 history 列表中的下一个 URL。

如果倒退之后，再想回到倒退之前浏览的页面，则可以使用forward()方法,代码如下:


```
window.history.forward();
```

注意：等价点击前进按钮。

forward()相当于go(1),代码如下:


```
window.history.go(1);
```

##### (3).返回浏览历史中的其他页面
go()方法，根据当前所处的页面，加载 history 列表中的某个具体的页面。


```
window.history.go(number);

go(1)等价forward()
go(0)等价当前页面
go(-1)等价back()
```

#### 3.Location对象
location用于获取或设置窗体的URL，并且可以用于解析URL。

location.[属性|方法]
location对象属性图示:



location 对象属性：



location 对象方法:



#### 4.Navigator对象
Navigator 对象包含有关浏览器的信息，通常用于检测浏览器与操作系统的版本。

对象属性:


#### 5.screen对象
screen对象用于获取用户的屏幕信息。

window.screen.属性
对象属性:



##### (1).屏幕分辨率的高和宽
window.screen 对象包含有关用户屏幕的信息。

1. screen.height 返回屏幕分辨率的高
1. screen.width 返回屏幕分辨率的宽

注意:

1.单位以像素计。

window.screen 对象在编写时可以不使用 window 这个前缀。

2.屏幕可用高和宽度
1. screen.availWidth 属性返回访问者屏幕的宽度，以像素计，减去界面特性，比如任务栏。
1. screen.availHeight 属性返回访问者屏幕的高度，以像素计，减去界面特性，比如任务栏。

注意:
> 不同系统的任务栏默认高度不一样，及任务栏的位置可在屏幕上下左右任何位置，所以有可能可用宽度和高度不一样。

### 六、DOM
文档对象模型DOM（Document Object Model）定义访问和处理HTML文档的标准方法。DOM 将HTML文档呈现为带有元素、属性和文本的树结构（节点树）。

HTML文档可以说由节点构成的集合，DOM节点有:



元素节点：上图中、、
等都是元素节点，即标签。

文本节点:向用户展示的内容，如…中的JavaScript、DOM、CSS等文本。

属性节点:元素属性，如标签的链接属性href=”http://www.imooc.com"。

节点属性:



遍历节点树:



DOM操作:



注意:前两个是document方法。

#### (1).getElementsByName()方法
注意:

1. 因为文档中的 name 属性可能不唯一，所有 getElementsByName() 方法返回的是元素的数组，而不是一个元素。
1. 和数组类似也有length属性，可以和访问数组一样的方法来访问，从0开始。

#### (2).getElementsByTagName()方法
返回带有指定标签名的节点对象的集合。返回元素的顺序是它们在文档中的顺序。

说明:

1. Tagname是标签的名称，如p、a、img等标签名。
1. 和数组类似也有length属性，可以和访问数组一样的方法来访问，所以从0开始。

##### (3).getAttribute()方法
通过元素节点的属性名称获取属性的值。


```
elementNode.getAttribute(name)
```


##### (4).setAttribute()
setAttribute() 方法增加一个指定名称和值的新属性，或者把一个现有的属性设定为指定的值。


```
elementNode.setAttribute(name,value)
```

说明：

1.name: 要设置的属性名。

2.value: 要设置的属性值。

注意：

1.把指定的属性设置为指定的值。如果不存在具有指定名称的属性，该方法将创建一个新属性。

2.类似于getAttribute()方法，setAttribute()方法只能通过元素节点对象调用的函数。

#### 5.节点属性
在文档对象模型 (DOM) 中，每个节点都是一个对象。DOM 节点有三个重要的属性 ：

1. nodeName : 节点的名称
1. nodeValue ：节点的值
1. nodeType ：节点的类型

一、nodeName 属性: 节点的名称，是只读的。

- 元素节点的 nodeName 与标签名相同
- 属性节点的 nodeName 是属性的名称
- 文本节点的 nodeName 永远是 #text
- 文档节点的 nodeName 永远是 #document
二、nodeValue 属性：节点的值

- 元素节点的 nodeValue 是 undefined 或 null
- 文本节点的 nodeValue 是文本自身
- 属性节点的 nodeValue 是属性的值
三、nodeType 属性: 节点的类型，是只读的。以下常用的几种结点类型:

元素类型 节点类型

- 元素 1
- 属性 2
- 文本 3
- 注释 8
- 文档 9


#### 6.访问子节点childNodes
访问选定元素节点下的所有子节点的列表，返回的值可以看作是一个数组，他具有length属性。


```
elementNode.childNodes
```

注意：
如果选定的节点没有子节点，则该属性返回不包含节点的 NodeList。




```
运行结果:
IE:
  UL子节点个数:3
  节点类型:1
其它浏览器:
   UL子节点个数:7
   节点类型:3
```

注意:

1. IE全系列、firefox、chrome、opera、safari兼容问题
1. 节点之间的空白符，在firefox、chrome、opera、safari浏览器是文本节点，所以IE是3，其它浏览器是7，如下图所示:

如果把代码改成这样:

javascriptjQueryPHP


```
运行结果:（IE和其它浏览器结果是一样的）
  UL子节点个数:3
  节点类型:1
```

#### 7.访问子节点的第一和最后项
一、firstChild 属性返回‘childNodes’数组的第一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。


```
node.firstChild
```

说明：与elementNode.childNodes[0]是同样的效果。

二、 lastChild 属性返回‘childNodes’数组的最后一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。


```
node.lastChild
```

说明：与elementNode.childNodes[elementNode.childNodes.length-1]是同样的效果。

#### 8.访问父节点parentNode
获取指定节点的父节点


```
elementNode.parentNode
```

注意:父节点只能有一个。

#### 9.访问兄弟节点
nextSibling 属性可返回某个节点之后紧跟的节点（处于同一树层级中）。


```
nodeObject.nextSibling
```


说明：如果无此节点，则该属性返回 null。

previousSibling 属性可返回某个节点之前紧跟的节点（处于同一树层级中）。


```
nodeObject.previousSibling
```


说明：如果无此节点，则该属性返回 null。

注意: 两个属性获取的是节点。Internet Explorer 会忽略节点间生成的空白文本节点（例如，换行符号），而其它浏览器不会忽略。

#### 10.插入节点appendChild()
在指定节点的最后一个子节点列表之后添加一个新的子节点。


```
appendChild(newnode)
```




```
运行结果:
HTML
JavaScript
This is a new p
```

#### 11.插入节点insertBefore()
insertBefore() 方法可在已有的子节点前插入一个新的子节点。


```
insertBefore(newnode,node);
```



```
运行结果:
This is a new p
JavaScript
HTML
```

#### 12.删除节点removeChild()
removeChild() 方法从子节点列表中删除某个节点。如删除成功，此方法可返回被删除的节点，如失败，则返回 NULL。


```
nodeObject.removeChild(node)
```




```
运行结果:
HTML
删除节点的内容: javascript
注意: 把删除的子节点赋值给 x，这个子节点不在DOM树中，但是还存在内存中，可通过 x 操作。
如果要完全删除对象，给 x 赋 null 值，代码如下:
```




#### 13.替换元素节点replaceChild()
replaceChild 实现子节点(对象)的替换。返回被替换对象的引用。


```
node.replaceChild (newnode,oldnew )
```



效果: 将文档中的 Java 改为 JavaScript。
注意:

1. 当 oldnode 被替换时，所有与之相关的属性内容都将被移除。
1. newnode 必须先被建立。
1. 只有父结点才能调用 replaceChild(newnode,oldnode).这个方法，所以，要想替换当前结点的内容或者属性，那么首先得获得父节点，才可以操作，这就是为什么 oldnode.parentNode.replaceChild(newnode,oldnode)

#### 14.创建元素节点createElement
createElement()方法可创建元素节点。此方法可返回一个 Element 对象。


```
document.createElement(tagName)
```

tagName：字符串值，这个字符串用来指明创建元素的类型。

注意：要与appendChild() 或 insertBefore()方法联合使用，将元素显示在页面中。

#### 15.创建文本节点createTextNode
createTextNode() 方法创建新的文本节点，返回新创建的 Text 节点。


```
document.createTextNode(data)
```



#### 16.浏览器窗口可视区域大小
获得浏览器窗口的尺寸（浏览器的视口，不包括工具栏和滚动条）的方法:

一、对于IE9+、Chrome、Firefox、Opera 以及 Safari：

• window.innerHeight - 浏览器窗口的内部高度

• window.innerWidth - 浏览器窗口的内部宽度

二、对于 Internet Explorer 8、7、6、5：

• document.documentElement.clientHeight表示HTML文档所在窗口的当前高度。

• document.documentElement.clientWidth表示HTML文档所在窗口的当前宽度。

或者

Document对象的body属性对应HTML文档的标签

• document.body.clientHeight

• document.body.clientWidth


```
在不同浏览器都实用的 JavaScript 方案：
var w= document.documentElement.clientWidth
      || document.body.clientWidth;
var h= document.documentElement.clientHeight
      || document.body.clientHeight;
```

#### 17.网页尺寸scrollHeight
scrollHeight和scrollWidth，获取网页内容高度和宽度。

一、针对IE、Opera:
scrollHeight 是网页内容实际高度，可以小于 clientHeight。

二、针对NS、FF:
scrollHeight 是网页内容高度，不过最小值是 clientHeight。也就是说网页内容实际高度小于 clientHeight 时，scrollHeight 返回 clientHeight 。

三、浏览器兼容性


```
var w=document.documentElement.scrollWidth
   || document.body.scrollWidth;
var h=document.documentElement.scrollHeight
   || document.body.scrollHeight;
```

注意:区分大小写
scrollHeight和scrollWidth还可获取Dom元素中内容实际占用的高度和宽度。

#### 18.网页尺寸offsetHeight
offsetHeight和offsetWidth，获取网页内容高度和宽度(包括滚动条等边线，会随窗口的显示大小改变)。

一、值

```
offsetHeight = clientHeight + 滚动条 + 边框。
```

二、浏览器兼容性


```
var w= document.documentElement.offsetWidth
    || document.body.offsetWidth;
var h= document.documentElement.offsetHeight
    || document.body.offsetHeight;
```

#### 19.网页卷去的距离与偏移量


scrollLeft:设置或获取位于给定对象左边界与窗口中目前可见内容的最左端之间的距离 ，即左边灰色的内容。

scrollTop:设置或获取位于对象最顶端与窗口中可见内容的最顶端之间的距离 ，即上边灰色的内容。

offsetLeft:获取指定对象相对于版面或由 offsetParent 属性指定的父坐标的计算左侧位置 。

offsetTop:获取指定对象相对于版面或由 offsetParent 属性指定的父坐标的计算顶端位置 。

注意:

区分大小写
offsetParent：布局中设置postion属性(Relative、Absolute、fixed)的父容器，从最近的父节点开始，一层层向上找，直到HTML的body。
