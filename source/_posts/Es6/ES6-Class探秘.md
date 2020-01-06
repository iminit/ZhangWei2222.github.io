---
title:  ES6-Class探秘
date: 2019-05-27 16:27:37
categories:
- Es6
comments: false
---

## 前言
总所周知，咱们传统的生成实例对象的方法是通过构造函数：

<!-- more -->

```javascript
function Point(x, y) {
    this.x = x;
    this.y = y;
}

Point.prototype.toString = funciton() {
    return `( ${this.x}, ${this.y} )`
}

var p = new Point(1, 2);
p.toString();
//( 1,2 )
```

现在 ES6 提供Class（类）的概念，一个语法糖，作为对象的模板。自从有了Class，咱们生成实例对象的写法就更加清晰，更像面对对象的语法。


```javascript
class Point {
    constructor (x, y) {
        this.x = x;
        this.y = y;
    }
    
    toString () {
        return `( ${this.x}, ${this.y} )` 
    }
}
var p = new Point(1, 2);
p.toString();
```

解析：类里面有个constructor方法——构造方法，this指向实例对象；咱们ES5的构造函数Point就相当于ES6的Point类的constructor方法

定义方法，也只需要直接写在类里面就好了。构造函数的prototype属性，在类上依然存在。（类上的所有方法都定义在类的prototype属性上）



##  严格模式（注意）

ES6里面默认是严格模式，只要代码写在类或模块里面，就只有严格模式可用哦。



## 深入Class的老窝

### constructor方法
类的默认方法，new生成实例对象时，自动调用。如果没有自定义，会默认加上空的constructor（所以是类里必须的方法）

默认返回实例对象（this），也可以指定返回另一个对象


```javascript
class Person {
    constructoe () {
        return Object.creat(null);
    }
}
new Person() instanceof Preson //false 判断实例是否为构造函数的实例
```



### 实例对象

生成的方式仍然是new命令。注意：实例的属性除非是定义在this对象（显示定义在本身，constructor方法），否则都是定义在原型(类）上


```javascript
class Point {
    constructor (x, y) {
        this.x = x;
        this.y = y;
    }
    
    toString () {
        return `( ${this.x}, ${this.y} )` 
    }
}
var p = new Point(1, 2);

Point.hasOwnProperty('x') //true
Point.hasOwnProperty('y') //true
Point.hasOwnProperty('toString') //false
Point.__proto__.hasOwnProperty('toString') //true
```



### 表达式

类可以用表达式的形式定义

```js
const MyClass = class Me {
    getClassName () {
        return Me.name;
    }
}
```

注意：这个类的名字叫MyClass而不是Me，Me只有在Class内部有定义

```js
let inst = new MyClass();
inst.getClassName(); //Me
Me.name //ReferenceError :Me is not defined
```

如果类的内部没有用到，可以省略Me


```javascript
const MyClass = class {
    getClassName () {
        return ;
    }
}
```

也可以写出立即执行Class

```javascript
let person = new class {
    constructor (name) {
        this.name = name;
    }
    
    sayName () {
        console.log(this.name);
    }
}('zhangwei');

person.sayName(); //zhangwei
```



### 不存在变量提升

即类使用在在，定义在后会报错的。原因和继承相关，必须保证子类在父类之后定义。



### this的指向

如果类里面的某个方法有this，默认指向类的实例，但是如果把这个方法单独使用，会指向该方法运行时的环境，一般会指到window

```javascript
class Test {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const test = new Test();

1、错误
const { printName } = test;
printName(); // TypeError: Cannot read property 'print' of undefined

2、正确
test.printName(); //Hello there
```



### getter和setter

可以在类内部使用get和set，对某个属性设置存值函数和取值函数，拦截该属性的存取行为


```javascript
class MyClass {
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123; // setter: 123

inst.prop // 'getter'
```



### 静态方法、静态属性、实例属性（重中之重）

类相当于实例的原型，所有类有的东西，都会被实例继承

#### 1、静态方法
- 类自己的方法，不会被实例继承

使用方法：**方法前面加 static关键字**（此方法可以被类调用）


```javascript
class Foo {
    static classMethod () {
        return 'hello';
    }
}

Foo.classMethod(); //hello

var foo = new Foo();
foo.classMethod(); // TypeError: foo.classMethod is not a function
```

注意1：静态方法的this指向类，而不是实例


```javascript
class Foo {
  static bar () {
    this.baz(); //this指的是Foo类，baz()为静态方法
  }
  static baz () { //静态方法可以与非静态方法重名
    console.log('hello');
  }
  baz () { 
    console.log('world');
  }
}

Foo.bar() // hello
```

注意2：父类的静态方法可以被子类继承；也可以从super对象上调用



#### 2、静态属性和实例属性

静态属性： 类自己的属性，不会被实例继承

因为ES6明确规定，class内没有静态属性，所以只有下面形式可行


```javascript
class Foo {}
Foo.prop = 1;
Foo.prop //1
```

但是ES7有一个提案：对静态属性和实例属性规定了新的写法。目前Babel转码器支持。

##### 静态属性
在属性前面加static关键字


```javascript
class Foo {
    static prop = 1;
}
```

##### 实例属性
之前的实例属性只能写在类的construtor方法里

```javascript
class ReactCounter extends React.Component {  
    constructor(props) {  
        super(props);  
        this.state = {  
            count: 0  
        };  
    }  
}
```
现在写法更清晰

```javascript
class ReactCounter extends React.Component {  
    state = {  
        count: 0  
    };  
}
```

并且在constructor里面定义的实例属性，还可以直接列出

```javascript
class ReactCounter extends React.Component {  
    constructor(props) {  
        super(props);  
        this.state = {  
            count: 0  
        };  
    }  
    state;
}
```

