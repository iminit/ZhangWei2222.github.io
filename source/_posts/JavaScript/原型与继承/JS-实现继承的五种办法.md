---
title: JS-实现继承的五种办法
date: 2019-09-10 10:12:37
categories:
- JavaScript
tags:
- 原型链
- this
comments: true
---

## 前言

之前一直对原型链不太了解，所以继承也懵懵懂懂，最近终于对原型链有点心得了，抓紧弄懂继承！

<!-- more -->

## 原型链继承

**核心：将父类的实例作为子类的原型**

**特点：**

- 实例 是子类的 实例，也是父类的 实例
- 父类新增原型方法/原型属性，子类都能访问到

**缺点：**

- 父类一变，子类也变
- 无法实现多继承：一个子类只能继承一个父类，不能继承多个父类
- 子类新增属性/方法，必须在new () 后面执行
- 创建子类实例时，无法向父类构造函数传参

```js
function Person(name) {
    this.name = 'wei' || name;
}
Person.prototype.getName = function() {   //对原型进行扩展
    return this.name;
}

function Parent(age) {
    this.age = age;
}
Parent.prototype = new Person();   //关键！！！通过构造器函数创建一个新对象，把老对象的东西拿过来

//注意，不要在父类构造函数传参，虽然语法上没啥问题，但是实际上并不符合 面向对象 编程规则：对象（实例）才是属性的拥有者
//如果创建子类实例时，传参，给属性赋值，就变成类拥有属性，而不是对象拥有属性
//例子： Parent.prototype = new Person('老明');
//此时，Parent类拥有了 name = '老明'属性，此后，它的实例对象 c1,c2,c3 都只能接受 name 这个属性

Parent.prototype.getAge = function() {
    return this.age;
}
//Parent.prototype.getName = function() {   //可以重写父类继承来的方法，会覆盖
//    console.log('222') ;
//}

var result = new Parent(22);
console.log(result.getName()); //wei
console.log(result.getAge());  //22
```



## 构造继承

**核心：在子类构造函数中利用 call/apply 把父类中通过 this 指定的属性/方法 复制到子类创建的实例中**

**特点：**

- 实例不是父类的实例，只是子类的实例
- 创建子类实例时，可以向父类传递参数
- 可以实现多继承（call 多个父类对象）

**缺点：**

- 只能继承父类的实例方法/属性，**不能继承父类原型**上的方法/属性
- 每个子类都有父类实例对象的副本，影响性能

```js
function Person (name) {
    this.name = name;
    this.friends = ['小李','小红'];
    this.getName = function () {
        return this.name;
    }
};

//Person.prototype.geSex = function () {    //对原型进行扩展的方法无法复用
//	console.log("男");
//};

function Parent = (age) {
    Person.call(this,'老明');　　//这一句是核心关键
    //这样就会在新parent对象上执行Person构造函数中定义的所有对象初始化代码，
    // 结果parent的每个实例都会具有自己的friends属性的副本
    this.age = age;
};

var result = new Parent(23);
console.log(result.name);　　　　//老明
console.log(result.friends);　　//["小李", "小红"]
console.log(result.getName());　　//老明
console.log(result.age);　　　　//23
console.log(result.getSex());　　//这个会报错，调用不到父原型上面扩展的方法
```



## 组合继承（最常用）

**核心：调用父类构造，继承父类的属性，保留传参的优点；将父类实例作为子类原型，实现父类原型 方法/属性 复用**

**特点：**

- 结合了 原型链继承 与 构造继承 的优点

**缺点：**

- 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

```js
function Person  (name) {
    this.name = name;
    this.friends = ['小李','小红'];
};

Person.prototype.getName = function () {
    return this.name;
};

function Parent (age) {
    Person.call(this,'老明');　　//这一步很关键
    this.age = age;
};

Parent.prototype = new Person('老明');　　//这一步也很关键
var result = new Parent(24);
console.log(result.name);　　　　//老明
result.friends.push("小智");　　//
console.log(result.friends);　　//['小李','小红','小智']
console.log(result.getName());　　//老明
console.log(result.age);　　　　//24

var result1 = new Parent(25);   //通过借用构造函数都有自己的属性，通过原型享用公共的方法
console.log(result1.name);　　//老明
console.log(result1.friends);　　//['小李','小红']
```



## 寄生组合继承

**核心：通过寄生方式，砍掉父类的实例属性。那么在调用两次父类的构造时，就不会初始化两次实例方法/熟悉，避免组合继承的缺点**

**优点：**

- 非常完美

**缺点：**

- 实现较复杂

```js
function Person(name) {
    this.name = name;
    this.friends = ['小李','小红'];
}

Person.prototype.getName = function () {
    return this.name;
};

function Parent(age) {
    Person.call(this,"老明"); //关键！！
    this.age = age;
}

(function () {  //关键！！
    var Super = function () {};     // 创建一个没有实例方法的类
    Super.prototype = Person.prototype;  //把类的原型指向父类的原型
    Parent.prototype = new Super();     //将实例作为子类的原型
})();

var result = new Parent(23);
console.log(result.name);
console.log(result.friends);
console.log(result.getName());
console.log(result.age);
```



## ES6 - class中的super()实现

核心：调用super方法：创造父类的实例对象this，然后再用子类的构造函数修改this

```js
class Animal { 
    constructor() { 　　
        this.type = 'animal'; 
    } 
    says(say) { 　　
        console.log(this.type + ' says ' + say); 
    } 
}   

let animal = new Animal(); 
animal.says('hello'); //animal says hello   

class Cat extends Animal { 　　
    constructor() { 　　　　
        super(); 　　　　
        this.type = 'cat'; 　　
    } 
}   
let cat = new Cat(); 
cat.says('hello'); //cat says hello 
```