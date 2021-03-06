---
title:  vue
date: 2019-03-07 15:32:37
categories:
- Vue
tags:
 - Vue-进阶
comments: false
---

### vue原理

Vue.js是一个构建数据驱动的 web 界面的渐进式框架。Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。核心是一个响应的数据绑定系统。

<!-- more -->

### 虚拟DOM

dom是树状结构，其节点为vnode，vnode和浏览器DOM中的Node一一对应，通过vnode的elm属性可以访问到对应的Node。

vdom因为是纯粹的JS对象，所以操作它会很高效，但是vdom的变更最终会转换成DOM操作，为了实现高效的DOM操作，一套高效的虚拟DOM diff算法显得很有必要。



用JS对象记录一个dom节点的副本，当dom发生更改时候，先用虚拟dom进行diff，算出最小差异，然后再修改真实dom。

diff算法是通过同层的树节点进行比较而非对树进行逐层搜索遍历的方式，所以时间复杂度只有O(n)，是一种相当高效的算法。



diff 的实现主要通过两个方法，patchVnode 与 updateChildren 。



patchVnode 有两个参数，分别是老节点 oldVnode, 新节点 vnode 。主要分五种情况：



if (oldVnode === vnode)，他们的引用一致，可以认为没有变化。

if(oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text)，文本节点的比较，需要修改，则会调用Node.textContent = vnode.text。

if( oldCh && ch && oldCh !== ch ), 两个节点都有子节点，而且它们不一样，这样我们会调用 updateChildren 函数比较子节点，这是diff的核心，后边会讲到。

if (ch)，只有新的节点有子节点，调用createEle(vnode)，vnode.el已经引用了老的dom节点，createEle函数会在老dom节点上添加子节点。

if (oldCh)，新节点没有子节点，老节点有子节点，直接删除老节点。

updateChildren 是关键，这个过程可以概括如下：

![img](http://s3.mogucdn.com/mlcdn/c45406/190613_0256ghk1i4bi55g1i92l7ad15e66f_725x571.png)

oldCh 和 newCh 各有两个头尾的变量 StartIdx 和 EndIdx ，它们的2个变量相互比较，一共有4种比较方式。如果 4 种比较都没匹配，如果设置了key，就会用key进行比较，在比较的过程中，变量会往中间靠，一旦 StartIdx > EndIdx 表明 oldCh 和 newCh 至少有一个已经遍历完了，就会结束比较。





### 为什么用vue

1.操作dom元素需要把html结构销毁之后,然后再进行重新生成,十分消耗性能

2.虚拟dom,通过diff算法,减少性能的消耗



vue通过建立一个虚拟DOM树对真实DOM发生的变化保持追踪。

一棵真实DOM树的渲染需要先解析CSS样式和DOM树，然后将其整合成一棵渲染树，再通过布局算法去计算每个节点在浏览器中的位置，最终输出到显示器上，

而虚拟DOM则可以理解为保存了一棵DOM树被渲染之前所包含的所有信息，而这些信息可以通过对象的形式一直保存在内存中，并通过JavaScript的操作进行维护。



**用传统jquery操作dom的思想，可以先删除,然后再插入新的标签（更新）**

#### 虚拟dom会如何处理上述问题呢？

第一步：通过树的形式保存旧的dom信息，这些信息可能在页面第一次加载的时候被渲染到浏览器中，但仍是通过虚拟dom的方式创建的

第二步：检测到数据更新，需要更新dom，先在JavaScript中将需要修改的节点全部修改完成（这个步骤可以进行算法上的优化，会在后面的原理中详细说明）

第三步：将最终生成的虚拟DOM更新到视图中去。

需要付出的代价付出的只是在你的内存中需要保存一份可供维护的数据信息





### vue特点

轻量级的框架、组件化、数据双向绑定



### VUE 响应式原理 data如何实现

第一步当然是通过 observer 进行数据劫持，然后在需要订阅的地方（如：模版编译），添加观察者（watcher），并立刻通过取值操作触发指定属性的 getter 方法，从而将观察者添加进 Dep （利用了闭包的特性，进行依赖收集），然后在 Setter 触发的时候，进行 notify，通知给所有观察者并进行相应的 update。



## 

#### Object.defineProperty()缺点

1、不能监听数组的变化

比如push pop shift等，不会触发setter，vue内部做了hack处理

2、必须遍历对象的每个属性

使用Object.defineProperty()多数要配合object.keys()和遍历，多了一层嵌套

3、必须深层遍历嵌套的对象



#### proxy

vue3.0的一个新特性

1、针对整个对象，而不是对象的某个属性，不用对keys进行便利（Reflect.get 和 Reflect.set 可以理解为类继承里的 super，即调用原来的方法）

2、支持数组

3、嵌套支持，在get里面递归调用proxy并返回

4、提供13种拦截的方法



### vue3.0 特性

1、虚拟dom重写，更多的编译时提示减少运行时开销。

2、优化slots的生成，目前在 Vue 中，当父组件重新渲染时，其子组件也必须重新渲染。使用Vue 3，可以单独重新渲染父级和子级。

3、静态树提升

Vue 3 的编译器将能够检测到什么是静态组件，然后将其提升，从而降低了渲染成本

4、静态属性提升

5、基于 Proxy 的 Observation

6、vue更小

7、更具有维护性，支持typescript，将内容更加模块化

8、新加observer模块



### Vue computed 实现

从两个问题出发：

建立与其他属性（如：data、 Store）的联系；

属性改变后，通知计算属性重新计算。

实现时，主要如下：



- 初始化 data， 使用 Object.defineProperty 把这些属性全部转为 getter/setter。

- 初始化 computed, 遍历 computed 里的每个属性，每个 computed 属性都是一个 watch 实例。每个属性提供的函数作为属性的 getter，使用 Object.defineProperty 转化。

- Object.defineProperty getter 依赖收集。用于依赖发生变化时，触发属性重新计算。

- 若出现当前 computed 计算属性嵌套其他 computed 计算属性时，先进行其他的依赖收集。



### computed里面用箭头函数的this是啥

undefined 因为继承上下文，不是vue实例



### computed & watch

computed,设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。

在页面中使用大量或是复杂的表达式去处理数据，对页面的维护会有很大的影响。这个时候就需要用到computed 计算属性来处理复杂的逻辑运算。

computed 计算属性只有在相关的数据发生变化时才会改变要计算的属性，当相关数据没有变化是，它会读取缓存。



watch，当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

watch模式没有computed模式简单，但watch比较适合做异步的操作。



### computed VS methods

不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。

**methods方式**是每次进入页面都要执行该方法，但是在利用实时信息时，比如显示当前进入页面的时间，必须用methods方式。



**computed**是在HTML DOM加载后马上执行的，如赋值；

methods则必须要有一定的触发条件才能执行，如点击事件；



### 为什么data要是一个function，不是一个普通的对象

因为组件是需要复用的，每次复用组件就会复制data，如果是普通的对象，那么每次复制的都是同一个地址的数据，那么更改了，都会更改。所以需要return一个新的对象，开辟一个新的空间，那么每个组件的data就是隔离的。



### 组件化 模块化

- 组件化：将实现页面某一部分功能的结构、样式和逻辑封装成一个整体，让它高内聚、低耦合，达到分治和复用的目的。
  - 比如页头、页尾，表单就是一个组件



- 模块化：是一种处理复杂系统分解成更好的可管理模块的方式。各模块通常都用一个标准的接口来进行通信。任何一个库实际上都是一个模块，比如node,log4js。任何语言都有模块化的思想，比如es6的import/export



### vue-router的实现机制大致是什么样，怎么去掉url里默认的#

vue-router通过hash和history两种方法实现前端路由，更新视图但不重新请求页面。



vue实现单页面应用，实际上是每一个页面都是一个单独的 vue实例，切换页面时，通过删除或不展示当前的实例，替换为新的实例来展示，实现浏览器不更新页面，在完成页面展示后修改地址栏，完成了不触发浏览器刷新，就替换了页面内容。



- hash方法：利用url的#，通过改变#后面hash值的变化，并不会导致浏览器向服务器发出请求，浏览器不发出请求，也就不会刷新页面。



- history方法:14年后，因为HTML5标准发布。多了两个 API，pushState 和 replaceState，通过这两个 API 可以改变 url 地址且不会发送请求。同时还有 popstate 事件。通过这些就能用另一种方式来实现前端路由了，但原理都是跟 hash 实现相同的。用了 HTML5 的实现，单页路由的 url 就不会多出一个#，变得更加美观。但因为没有 # 号，所以当用户刷新页面之类的操作时，浏览器还是会给服务器发送请求。



将mode改成history。默认是hash



### mvvm



MVVM是Model-View-ViewModel的简写。即模型-视图-视图模型。在MVVM的框架下视图和模型是不能直接通信的。它们通过ViewModel来通信，ViewModel通常要实现一个observer观察者，当数据发生变化，ViewModel能够监听到数据的这种变化，然后通知到对应的视图做自动更新，而当用户操作视图，ViewModel也能监听到视图的变化，然后通知数据做改动，这实际上就实现了数据的双向绑定。并且MVVM中的View 和 ViewModel可以互相通信。MVVM流程图如下：

![img](http://s3.mogucdn.com/mlcdn/c45406/190613_3je0dba35cj7h381c41k02j93ajk6_500x100.png)



### mvc

MVC是Model-View- Controller的简写。即模型-视图-控制器。M和V指的意思和MVVM中的M和V意思一样。C即Controller指的是页面业务逻辑。使用MVC的目的就是将M和V的代码分离。‘MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下。MVC和MVVM的区别并不是VM完全取代了C，ViewModel存在目的在于抽离Controller中展示的业务逻辑，而不是替代Controller，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重用。



- 视图（View）：用户界面。（传送指令到 Controller）
- 控制器（Controller）：业务逻辑（完成业务逻辑后，要求 Model 改变状态）
- 模型（Model）：数据保存（将新的数据发送到 View，用户得到反）

![img](http://s11.mogucdn.com/mlcdn/c45406/190613_591chg93g8489l2fg6k2h93k3e5a3_601x365.png)

MVC允许在不改变视图的情况下改变视图对用户输入的响应方式，用户对View的操作交给了Controller处理，在Controller中响应View的事件调用Model的接口对数据进行操作，一旦Model发生变化便通知相关视图进行更新。

提交一次反馈一次，通信一次相互制约。









### vue几种常用的指令

答：v-for 、 v-if 、v-bind、v-on、v-show、v-else



相同点：v-if与v-show都可以动态控制dom元素显示隐藏

不同点：v-if显示隐藏是将dom元素整个添加或删除，而v-show隐藏则是为该元素添加css--display:none，dom元素还在。

**1.手段**：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐；

**2.编译过程**：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；

**3.编译条件**：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留；

**4.性能消耗：**v-if有更高的切换消耗；v-show有更高的初始渲染消耗；

**5.使用场景：**v-if适合运营条件不大可能改变；v-show适合频繁切换。


### 聊聊你对Vue.js的template编译的理解？

答：简而言之，就是先转化成AST树，再得到的render函数返回VNode（Vue的虚拟DOM节点）

详情步骤：

首先，通过compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option。

然后，AST会经过generate（将AST语法树转化成render funtion字符串的过程）得到render函数，render的返回值是VNode，VNode是Vue的虚拟DOM节点，里面有（标签名、子节点、文本等等）