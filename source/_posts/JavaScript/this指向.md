---
title: this指向
date: 2019-03-01 10:05:48
categories:
- JavaScript
tags:
- this
comments: false
---

## 普通的this指向
this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象
- 如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window
- 如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象
- 如果一个函数中有this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象

<!-- more -->

## 构造函数的this指向

```js
function Foo(name,age){
  this.name=name;
  this.age=age;
}
var foo=new Foo("Tom",14);
foo.name;//Tom
foo.age;//14
```

构造函数的 this **指向实例的**，因为 **new 的步骤**

- 创建一个新对象
- 把构造函数的作用域赋给新对象，因此 this 指向新对象 
- 新对象的 _proto_ 属性指向构造函数的prototype原型
- 执行构造函数的代码
- 返回新对象



## 当this碰到return

```js
function Foo(name,age){
  this.name=name;
  this.age=age;
  return {name:"Jeff"}
}
var foo=new Foo("Tom",14);
foo.name;//Jeff
```

如果返回值是一个对象({},[],RegExp, Date, Function)，那么this指向的就是那个返回的对象，如果返回值不是一个对象(基本类型)那么this还是指向函数的实例



## 箭头函数的this指向

- 箭头函数本身没有this，不能用call()、apply()、bind()这些方法去改变this的指向
- 箭头函数中的this是继承 父执行上下文 中的this，在定义函数的时候绑定，而不是在执行函数的时候绑定

```js
var x=11;
var obj={
 x:22,
 say:()=>{
   console.log(this.x);
 }
}
obj.say();
//输出的值为11
```

比如这里的箭头函数中的this.x，箭头函数本身与say平级以key:value的形式，也就是箭头函数本身所在的对象为obj，而obj的父执行上下文就是window，因此这里的this.x实际上表示的是window.x，因此输出的是11



## call,apply,bind方法的总结

#### 用途
call 、apply 、bind 三者都是用来改变函数的this对象的指向的

#### call、apply 区别
- call 接收的是参数列表
- apply 接收的是参数数组
- 参数是传给调用的对象的

```js
var a ={
  name : "Cherry",
  fn : function (a,b) {
   console.log( a + b)
  }
 }
 var b = a.fn;
 b.call(a,1,2)  // 3

var a ={
  name : "Cherry",
  fn : function (a,b) {
   console.log( a + b)
  }
 }
 var b = a.fn;
 b.apply(a,[1,2])  // 3
```
#### call、apply 与 bind 区别
call 和 apply 都是改变上下文中的this并立即执行这个函数，bind方法可以让对应的函数想什么时候调就什么时候调用

```js
var a ={
  name : "Cherry",
  fn : function (a,b) {
   console.log( a + b)
  }
 }
var b = a.fn;
b.bind(a,1,2)()   // 3
---  或  ---
var c = b.bind(a,1,2)
c //注意，返回的是一个函数
//ƒ (a,b) {
// console.log( a + b)
//}
c() // 3
```

#### 多次绑定 bind 只有第一次是有效的

```js
var one = function(){
	console.log(this.x);
}
var two = {
	x: 1
}
var three = {
	x: 2
}
var fn = one.bind(two).bind(three);
fn(); //1
```

因为 bind() 的实现，相当于使用函数在内部包了一个 call/apply，第二次 bind() 相当于再包住第一次 bind()，所以后面的 bind 都是无效的

```js
var fn = function() {
    return function() {
    	one.apply(two);
    }.apply(three);
}
```

#### 利用 call() 和 apply() 方法实现 bind() 方法

```js
if( !function(){}.bind ) {
    Function.prototype.bind = function(obj) {
        var self = this; //this 是调用函数的对象
        var args = Array.prototype.slice.call(arguments); //将 arguments 类数组转换成数组
        
        return function() {
            return self.apply(obj, args.slice(1));
        }
    }
}
```

