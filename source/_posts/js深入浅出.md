---
title: js深入浅出
date: 2017-08-18 15:15:47
categories:
- 前端技术篇
tags:
- javaScript
comments: false
---


### 一.数据类型
原始类型
> number
> 
> string
> 
> boolean
> 
> null
> 
> undefined
> 
object 对象
> Function
> 
> Array
> 
> Date
> 

#### 2.隐式转换
字符串变数字

> num - 0

数字变字符串

> num + ‘’


```
37” – 7 // 30
“37” + 7 // 377
```

**a===b**

类型不同

> 返回false

类型相同

> NaN ≠ NaN
> 
> new Object ≠ new Object
> 
> null === null
> 
> undefined === undefined

**a == b**

类型相同

> 同===

类型不同，尝试类型转换和比较:

> null == undefined 相等
> 
> number == string 转number
> 1 == “1.0” // true
> 
> boolean == ? 转number
> 1 == true // true
> 
> object == number | string
> 尝试对象转为基本类型
> new String(‘hi’) == ‘hi’ // true
> 
> 其它：false

#### 3.包装对象

```
var a = “string”;
alert(a.length);
a.t = 3;
alert(a.t);
```

当对一个字符串（或数组或数字或布尔）（基本类型）进行对象访问的时候，会创建一个对象，当访问完毕，对象立刻销毁，所以a.t设置完后，不存在

#### 4.类型检测
**typeof**

```
typeof 100 === “number”
typeof true === “boolean”
typeof function () {} === “function”

typeof(undefined) ) === “undefined”
typeof(new Object() ) === “object”
typeof( [1， 2] ) === “object”
typeof(NaN ) === “number”
**typeof(null) === “object”**
```

**obj instanceof Object**

```
[1, 2] instanceof Array === true
new Object() instanceof Array === false
```

Instanceof运算符的第一个变量是一个对象，暂时称为A；第二个变量一般是一个函数，暂时称为B。

Instanceof的判断队则是：沿着A的proto这条线来找，同时沿着B的prototype这条线来找，如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。



Caution！不同window或iframe间的对象类型检测不能使用instanceof！

**Object.prototype.toString**

```
Object.prototype.toString.apply([]); === “[object Array]”;
Object.prototype.toString.apply(function(){}); === “[object Function]”;
Object.prototype.toString.apply(null); === “[object Null]”
Object.prototype.toString.apply(undefined); === “[object Undefined]”

IE6/7/8 Object.prototype.toString.apply(null) 返回”[object Object]”
```

**constructor**
constructor会指向构造这个对象的构造器或构造函数，constructor可以改写，使用时要小心

**duck type**
通过一些特征来判断

**类型检测小结**
> typeof
> 
> 适合基本类型及function检测，遇到null失效。
> 
> [[Class]]
> 
> 通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效(IE678等返回[object Object])。
> 
> instanceof
> 
> 适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。

### 二.表达式和运算符
表达式是一种JS短语，可使JS解释器用来产生一个值。

#### 表达式
#### 1.原始表达式
> 1.常量、直接量
> 
> 3.14, “test”
> 
> 2.关键字
> 
> null, this, true
> 
> 3.变量
> 
> i, k, j
> 
> 复合表达式 10*20

#### 2.数组、对象的初始化表达式
> [1, 2]
> 
> new Array(1, 2);
> 
> [1, , , 4]
> 
> [1, undefined, undefined , 4]
> 
> { x : 1, y : 2}
> 
> var o = new Object();
> o.x = 1; o.y = 2;

#### 3.函数表达式

```
var fe = function(){};
```


#### 4.属性访问表达式

```
var o = {x : 1};
```


> o.x
> 
> o[‘x’]

#### 5.调用表达式

```
func();
```


#### 6.对象创建表达式

```
new Func(1, 2);
new Object;
```


#### 运算符
一元 +num

二元 a + b

三元 c ? a : b

赋值 x += 1

比较 a == b

算术 a - b

位 a | b

逻辑 exp1 && exp2

字符串 “a” + “b”

特殊

#### 1.delete obj.x

```
var obj = {x : 1};
obj.x;  // 1
delete obj.x;
obj.x;  // undefined

var obj = {};
Object.defineProperty(obj, 'x', {
    configurable : false, 
    value : 1
});
delete obj.x; // false
obj.x;// 1
```

神奇的Object.defineProperty


```
var a={}
Object.defineProperty(a,"b",{value:123})
console.log(a,b);//123
```

传入参数

> 第一个参数:目标对象
> 
> 第二个参数:需要定义的属性或方法的名字。
> 
> 第三个参数:目标属性所拥有的特性。(默认为false)（descriptor）

前两个参数不多说了，一看代码就懂，主要看第三个参数descriptor，看看有哪些取值

value:属性的值(不用多说了)


```
var a= {}
Object.defineProperty(a,"b",{
  value:123
})
console.log(a.b);//123
```

writable:如果为false，属性的值就不能被重写,只能为只读了

```
var a = {};

Object.defineProperty(o, "b", { 
value : 123,
writable : false });

console.log(a.b); // 打印 37
a.b = 25; // 没有错误抛出（在严格模式下会抛出，即使之前已经有相同的值）
console.log(o.a); // 打印 37， 赋值不起作用。
```

configurable:总开关，一旦为false，就不能再设置他的（value，writable，configurable）

enumerable:是否能在for…in循环中遍历出来或在Object.keys中列举出来。


```
var a= {}
Object.defineProperty(a,"b",{
  value:3445,
  enumerable:true
})
console.log(Object.keys(a));// 打印["b"]
改为false

var a= {}
Object.defineProperty(a,"b",{
  value:3445,
  enumerable:false //注意咯这里改了
})
console.log(Object.keys(a));// 打印[]
```

get:一会细说

set:一会细说

#### 2.in

```
window.x = 1;
‘x’ in window; // true
```

#### 3.instanceof, typeof

```
{} instanceof Object// true
typeof 100 === ‘number’ // true
```

#### 4.new

```
function Foo(){}
Foo.prototype.x = 1;
var obj = new Foo();
obj.x;  // 1
obj.hasOwnProperty('x'); // false
obj.__proto__.hasOwnProperty('x'); // true
```

> hasOwnProperty 本身的属性
> 
> proto即原型，obj这个对象的原型指向Foo这个函数的prototype

#### 5.this

```
this;  // window (浏览器)
var obj = {
func : function(){return this;}
};
obj.func(); // obj
```

#### 6.void

```
void 0  // undefined
void(0) // undefined
```

### 三.语句
#### 1.block
请注意：没有块级作用域


```
var i = 0
for (; i < 10; i++) {
var str = "hi";
console.log(str);
}
```

为了防止混淆，把for循环中的定义变量var，拿出去写

#### 2.var

```
var a = b = 1;
```

b隐式创建了全局变量（所以一条语句定义多个变量，一定要分开）

#### 3.try catch

```
try {
   throw "test";
} catch (ex) {
console.log(ex); // test
} finally {
console.log('finally');
}
```

先执行try catch中的代码，如果抛出了异常，会由catch从句捕获并且执行，如果没有发生异常，catch部分的代码被忽略掉，但不管是否出现异常，都会执行finally从句；try后面必须接一个catch或一个finally


```
try {
try {
throw new Error("oops");
}
finally {
console.log("finally");
}
}
catch (ex) {
console.error("outer", ex.message);
}

"finally"
"outer" "oops"
try {
  try {
  throw new Error("oops");
  }
  catch (ex) {
console.error("inner", ex.message);
  }
  finally {
console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

"inner" "oops"
"finally"
try {
  try {
throw new Error("oops");
  }
  catch (ex) {
console.error("inner", ex.message);
throw ex;
  }
  finally {
console.log("finally");
  }
}
catch (ex) {
  console.error("outer", ex.message);
}

"inner" "oops"
"finally"
"outer" "oops"
```

#### 4.function

```
fd(); // true
function fd() {
// do sth.
return true;
}
```

函数声明，会被前置处理掉


```
fe(); // TypeError
var fe = function() {
// do sth.
};
```

函数表达式，不会提前处理

#### 5.for…in

```
var p;
var obj = {x : 1, y: 2}

for (p in obj) {
}
```

1. 顺序不确定
1. enumerable为false时不会出现
1. for in对象属性时受原型链影响

#### 6.switch
#### 7.循环
#### 8.with

```
with ({x : 1}) {
console.log(x);
}

with (document.forms[0]) {
console.log(name.value);
}

var form = document.forms[0];
console.log(form.name.value);
```

通俗的理解就是，with代表了那个对象。可是这样写有几个缺点：（因此，应该采用变量取对象的方法）

让JS引擎优化更难

可读性差

可被变量定义代替

严格模式下被禁用

修改当前的作用域

#### 严格模式
严格模式是一种特殊的执行模式，
它修复了部分语言上的不足，提供更强的错误检查，并增强安全性。


```
function func() {
'use strict';
}

 'use strict';
function func() {

}
```

#### 1.不允许用with

```
!function() {
    'use strict';
     with({x : 1}) {
console.log(x);
  }
}(); //SyntaxError
```

#### 2.不允许未声明的变量被赋值

```
!function() {
    'use strict';
     x = 1;
  console.log(window.x);
}(); //ReferenceError
```

#### 3.arguments变为参数的静态副本

```
!function(a) {
    arguments[0] = 100;
    console.log(a);
}(1);  //100
```

在一般模式下，如果定义了一个函数，调用它并且传递参数，那么它对应的形参和arguments[0]有一个相互的绑定关系，就是说如果我们修改了arguments[0]，那么这个函数对应的形参b就会被修改了，所以输出100。

如果我们现在不给函数传值，就是像下面这个样子，


```
! function (a){
arguments[0]=100;
console.log(a);//undefined
  }();
```

那么无论我们如何修改arguments[0]的值，输出的都是undefined，因为宝宝你没有传递参数进去啊，~ o(￣▽￣)ブ

!function(a) {
    'use strict';
    arguments[0] = 100;
    console.log(a);
}(1);  //1
在严格模式下，arguments[0]变为参数的静态副本，就是说无论函数的参数有没有传递，都不会和arguments相互影响


```
!function(a) {
'use strict';
arguments[0].x = 100;
console.log(a.x);
}({x:1});  //100
```

但是如果传入的是一个对象的话，将会按照共享传递，使用arguments修改对象的属性，那么还是会相互影响的。

> arguments用法
> 
> 每个函数都会有一个Arguments对象实例arguments，它引用着函数的实参，可以用数组下标的方式”[]”引用arguments的元素。arguments.length为函数实参个数，arguments.callee引用函数自身。


```
var sum = function (n) {
if (1 == n) {
return 1;
} else {
return n + arguments.callee(n - 1);
}
}
alert(sum(6));
```

#### 4.delete参数、函数名报错

```
!function(a) {
    'use strict';
    delete a;
}(1); //SyntaxError
```

#### 5.delete不可配置的属性报错

```
!function(a) {
    'use strict';
    var obj = {};
    Object.defineProperty(obj, 
    'a', {configurable : false});
    delete obj.a;
}(1); //TypeError
```

#### 6.对象字面量重复属性名报错

```
!function() {
    'use strict';
    var obj = {x : 1, x : 2};
}();  //SyntaxError
```

#### 7.禁止八进制字面量

```
!function() {
    'use strict';
    console.log(0123);
}(); //SyntaxError
```

#### 8.eval, arguments变为关键字，不能作为变量、函数名

```
!function() {
    'use strict';
    function eval(){}
}();  //SyntaxError
```

#### 9.eval独立作用域

```
!function() {
    'use strict';
    eval('var evalVal = 2;');
    console.log(typeof evalVal);
}();  //undefined
```

> 小结
> 
> 1.不允许用with
> 
> 2.所有变量必须声明, 赋值给为声明的变量报错，而不是隐式创建全局变量。
> 
> 3.eval中的代码不能创建eval所在作用域下的变量、函数。而是为eval单独创建一个作用域，并在eval返回时丢弃。
> 
> 4.函数中得特殊对象arguments是静态副本，而不像非严格模式那样，修改arguments或修改参数变量会相互影响。
> 
> 5.删除configurable=false的属性时报错，而不是忽略
> 
> 6.禁止八进制字面量，如010 (八进制的8)
> 
> 7.eval, arguments变为关键字，不可作为变量名、函数名等
> 
> 8.一般函数调用时(不是对象的方法调用，也不使用apply/call/bind等修改this)this指向null，而不是全局对象。
> 
> 9.若使用apply/call，当传入null或undefined时，this将指向null或undefined，而不是全局对象。
> 
> 10.试图修改不可写属性(writable=false)，在不可扩展的对象上添加属性时报TypeError，而不是忽略。
> 
> 11.arguments.caller, arguments.callee被禁用

### 四.对象
对象中包含一系列属性，这些属性是无序的。
每个属性都有一个字符串key和对应的value。


```
var obj = {x : 1, y : 2};
obj.x; // 1
obj.y; // 2
```

对象创建-字面量

```
var obj1 = {x : 1, y : 2};
var obj2 = {
x : 1,
y : 2,
o : {
z : 3,
n : 4
}
};
```


#### 1.创建对象-new/原型链

```
function foo(){}
foo.prototype.z = 3;

var obj =new foo();
obj.y = 2;
obj.x = 1;

obj.x; // 1
obj.y; // 2
obj.z; // 3
typeof obj.toString; // ‘function'
'z' in obj; // true
obj.hasOwnProperty('z'); // false
obj.z不在obj中，会向上查找，即沿着原型链

obj.z = 5;

obj.hasOwnProperty('z'); // true
foo.prototype.z; // still 3
obj.z; // 5

obj.z = undefined;
obj.z; // undefined

delete obj.z; // true
obj.z; // 3

delete obj.z; // true
**obj.z; // still 3!!!**
不能delete掉原型链的属性z
```


#### 2.对象创建-Object.create

```
var obj = Object.create({x : 1});
obj.x // 1
typeof obj.toString // "function"
obj.hasOwnProperty('x');// false
```

创建一个新对象，并将这个对象的原型指向Object.propotype，create后面的属性属于object.propotype中。


```
var obj = Object.create(null);
obj.toString // undefined
```

#### (2)属性操作
#### 1.属性读写

```
var obj = {x : 1, y : 2};
obj.x; // 1
obj["y"]; // 2

obj["x"] = 3;
obj.y = 4;

var obj = {x1 : 1, x2 : 2};
var i = 1, n = 2;

for (; i <= n; i++) {
console.log(obj['x' + i]);
}
// 输出: 1, 2

var p;
for (p in obj) {
console.log(obj[p]);
}
```

#### 2.属性读写-异常

```
var obj = {x : 1};
obj.y; // undefined
var yz = obj.y.z; // TypeError: Cannot read property 'z' of undefined
obj.y.z = 2; // TypeError: Cannot set property 'z' of undefined

var yz;
if (obj.y) {
    yz = obj.y.z;
}

var yz = obj && obj.y && obj.y.z;
```

#### 3.属性删除

```
var person = {age : 28, title : 'fe'};
delete person.age; // true
delete person['title']; // true
person.age; // undefined
delete person.age; // true

delete Object.prototype; // false,

var descriptor = Object.getOwnPropertyDescriptor(Object, 'prototype');
descriptor.configurable; // false
```

不能delete Object.prototype


```
var globalVal = 1;
delete globalVal; // false

(function() {
    var localVal = 1;
    return delete localVal;
}()); // false
```

不能delete 变量


```
function fd() {}
delete fd; // false

(function() {
    function fd() {};
    return delete fd;
}()); // false
```

不能delete 函数


```
ohNo = 1;
window.ohNo; // 1
delete ohNo; // true
```

可以delete 全局变量

#### 4.属性检测

```
var cat = new Object;
cat.legs = 4;
cat.name = "Kitty";

'legs' in cat; // true
'abc' in cat; // false
"toString" in cat; // true, inherited property!!!


cat.hasOwnProperty('legs'); // true
cat.hasOwnProperty('toString'); // false

cat.propertyIsEnumerable('legs'); // true
cat.propertyIsEnumerable('toString'); // false
```

如果判断的属性存在于Object对象的原型内，不管它是否可枚举都会返回false。


```
Object.defineProperty(cat, 'price', {enumerable : false, value : 1000});
cat.propertyIsEnumerable('price'); // false
cat.hasOwnProperty('price'); // true


if (cat && cat.legs) {
    cat.legs *= 2;
}


if (cat.legs !== undefined) {
    // only if cat.legs is not undefined
}
```

#### 5.getter setter方法(另一种读写属性的方式)
getter 是一种获得属性值的方法，setter是一种设置属性值的方法。


```
var man = {
    name : 'Bosn',
    weibo : '@Bosn',
    get age() {
        return new Date().getFullYear() - 1988;
    },
    set age(val) {
        console.log('Age can\'t be set to ' + val);
    }
}
console.log(man.age); // 27
man.age = 100; // Age can't be set to 100
console.log(man.age); // still 27
var man = {
    weibo : '@Bosn',
    $age : null,
    get age() {
        if (this.$age == undefined) {
            return new Date().getFullYear() - 1988;
        } else {
            return this.$age;
        }
    },
    set age(val) {
        val = +val;
        if (!isNaN(val) && val > 0 && val < 150) {
            this.$age = +val;
        } else {
            throw new Error('Incorrect val = ' + val);
        }
    }
}
console.log(man.age); // 27
man.age = 100;
console.log(man.age); // 100;
man.age = 'abc'; // error:Incorrect val = NaN
get/set与原型链

function foo() {}

Object.defineProperty(foo.prototype, 'z', 
    {get : function(){return 1;}});

var obj = new foo();

obj.z; // 1
obj.z = 10;
obj.z; // still 1

Object.defineProperty(obj, 'z', 
{value : 100, configurable: true});
obj.z; // 100;
delete obj.z;
obj.z; // back to 1
不能修改get的属性
```


#### (3)属性标签

```
Object.getOwnPropertyDescriptor({pro : true}, 'pro');
// Object {value: true, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor({pro : true}, 'a'); // undefined
```

> Object.getOwnPropertyDescriptor 函数
> 
> 语法：Object.getOwnPropertyDescriptor(object, propertyname)
> 
> 参数：
> object 必需。包含属性的对象。
> propertyname 必需。属性的名称。


```
var person = {};
Object.defineProperty(person, 'name', {
    configurable : false,
    writable : false,
    enumerable : true,
    value : "Bosn Ma"
});

person.name; // Bosn Ma
person.name = 1;
person.name; // still Bosn Ma
delete person.name; // false
```

defineProperty中的属性默认false


```
Object.defineProperty(person, 'type', {
    configurable : true,
    writable : true,
    enumerable : false,
    value : "Object"
});

Object.keys(person); // ["name"]
Object.defineProperties(person, {
    title : {value : 'fe', enumerable : true},
    corp : {value : 'BABA', enumerable : true},
    salary : {value : 50000, enumerable : true, writable : true}
});

Object.getOwnPropertyDescriptor(person, 'salary');
// Object {value: 50000, writable: true, enumerable: true, configurable: false}
Object.getOwnPropertyDescriptor(person, 'corp');
// Object {value: "BABA", writable: false, enumerable: true, configurable: false}
Object.defineProperties(person, {
    title : {value : 'fe', enumerable : true},
    corp : {value : 'BABA', enumerable : true},
    salary : {value : 50000, enumerable : true, writable : true},
    luck : {
    get : function() {
    return Math.random() > 0.5 ? 'good' : 'bad';
    }
    },
    promote : {
    set : function (level) {
    this.salary *= 1 + level * 0.1;
    }
   }
});

Object.getOwnPropertyDescriptor(person, 'salary');
// Object {value: 50000, writable: true, enumerable: true, configurable: false}
Object.getOwnPropertyDescriptor(person, 'corp');
// Object {value: "BABA", writable: false, enumerable: true, configurable: false}
person.salary; // 50000
person.promote = 2;
person.salary; // 60000
```
getOwnPropertyDescriptor属性值默认false




> 1.configurable为false时，不能delete,不能修改getter/setter方法，不能修改属性标签
> 
> 2.writable为false时，不能通过属性赋值，修改属性的值
> 
> 3.configurable和writeable都是false时，才不能修改属性的值

#### （4）.对象标签

#### 1.原型标签 _proto_


#### 2.class标签

```
var toString = Object.prototype.toString;
function getType(o){return toString.call(o).slice(8,-1);};

toString.call(null); // "[object Null]"
getType(null); // "Null"
getType(undefined); // "Undefined"
getType(1); // "Number"
getType(new Number(1)); // "Number"
typeof new Number(1); // "object"
getType(true); // "Boolean"
getType(new Boolean(true)); // "Boolean"
```

> slice() 方法
> 
> 定义和用法
> slice() 方法可从已有的数组中返回选定的元素。
> 
> 语法
> arrayObject.slice(start,end)
> 
> start
> 必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
> 
> end
> 可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。
> 
> 返回值
> 返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素。
> 
> 说明
> 请注意，该方法并不会修改数组，而是返回一个子数组。

#### 3.extensible标签

**扩展特性**
> 如果一个对象可以添加新的属性，则这个对象是可扩展的。让这个对象变的不可扩展，也就是不能再有新的属性


```
var obj = {x : 1, y : 2};
Object.isExtensible(obj); // true
```

> 对象的extensible属性用于表示是否允许在对象中动态添加新的property。ECMAScript 3标准中，所有的对象都是extensible的。


```
Object.preventExtensions(obj);
Object.isExtensible(obj); // false
```

使一个对象变成不是extensible的，可以使用Object.preventExtensions()语句


```
obj.z = 1;
obj.z; // undefined, add new property failed
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: true, enumerable: true, configurable: true}
```

**密封特性**
密封对象是指那些不可 扩展 的，且所有自身属性都不可配置的（non-configurable）对象。

或则说 密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象


```
Object.seal(obj);
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: true, enumerable: true, configurable: false}
Object.isSealed(obj); // true
```

**冻结特性**
冻结对象是指那些不能添加新的属性，不能修改已有属性的值，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性的对象。也就是说，这个对象永远是不可变的。


```
Object.freeze(obj);
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: false, enumerable: true, configurable: false}
Object.isFrozen(obj); // true

// [caution] not affects prototype chain!!!
```

#### (5).序列化
v
```
ar obj = {x : 1, y : true, z : [1, 2, 3], nullVal : null};
JSON.stringify(obj); // "{"x":1,"y":true,"z":[1,2,3],"nullVal":null}"

obj = {val : undefined, a : NaN, b : Infinity, c : new Date()};
JSON.stringify(obj); // "{"a":null,"b":null,"c":"2015-01-20T14:15:43.910Z"}"

obj = JSON.parse('{"x" : 1}');
obj.x; // 1
var obj = {
    x : 1,
    y : 2,
    o : {
        o1 : 1,
        o2 : 2,
        toJSON : function () {
            return this.o1 + this.o2;
        }
    }
};
JSON.stringify(obj); // "{"x":1,"y":2,"o":3}"
stringify()用于从一个对象解析出字符串，如

var
 a = {a:1,b:2}

结果：

JSON.stringify(a)

"{"a":1,"b":2}"
parse用于从一个字符串中解析出json对象,如

var str = '{"name":"huangxiaojian","age":"23"}'

结果：

JSON.parse(str)

Object

age: "23"
name: "huangxiaojian"


__proto__: Object
注意：单引号写在{}外，每个属性名都必须用双引号，否则会抛出异常。
```

#### （5）.其他对象方法

```
var obj = {x : 1, y : 2};
obj.toString(); // “[object Object]”
obj.toString = function() {return this.x + this.y};
“Result “ + obj; // “Result 3”, by toString

+obj; // 3, from toString

obj.valueOf = function() {return this.x + this.y + 100;};
+obj; // 103, from valueOf

"Result " + obj; // still "Result 103"
```

> object.toString( )
> 
> 返回值 toString()函数的返回值为String类型。返回当前对象的字符串形式。
> 
> Array
> 将 Array 的每个元素转换为字符串，并将它们依次连接起来，两个元素之间用英文逗号作为分隔符进行拼接。
> 
> Boolean
> 如果布尔值是true，则返回”true”。否则返回”false”。
> 
> Date
> 返回日期的文本表示。
> 
> Error
> 返回一个包含相关错误信息的字符串。
> 
> Function
> 返回如下格式的字符串，其中 functionname 是一个函数的名称，此函数的 toString 方法被调用： “function functionname() { [native code] }”
> 
> Number
> 返回数值的字符串表示。还可返回以指定进制表示的字符串
> 
> String
> 返回 String 对象的值。
> 
> Object(默认)
> 返回”[object ObjectName]”，其中 ObjectName 是对象类型的名称。
> 
> object.valueOf( )
> 
> 返回值 函数返回指定对象的原始值。
> 
> Array
> 数组实例对象。
> 
> Boolean
> 布尔值。
> 
> Date
> 以毫秒数存储的时间值，从 UTC 1970 年 1 月 1 日午夜开始计算。
> 
> Function
> 函数本身。
> 
> Number
> 数字值。
> 
> Object
> 对象本身。这是默认设置。
> 
> String
> 字符串值。


```
var bbb = { 
i: 10, 
toString: function() { 
console.log('toString'); 
return this.i; 
}, 
valueOf: function() { 
console.log('valueOf'); 
return this.i; 
} 
} 
alert(bbb);// 10 toString 
alert(+bbb); // 10 valueOf 
alert(''+bbb); // 10 valueOf 
alert(String(bbb)); // 10 toString 
alert(Number(bbb)); // 10 valueOf 
alert(bbb == '10'); // true valueOf 
alert(bbb === '10'); // false 
var aa = { 
i: 10, 
toString: function() { 
console.log('toString'); 
return this.i; 
} 
} 
alert(aa);// 10 toString 
alert(+aa); // 10 toString 
alert(''+aa); // 10 toString 
alert(String(aa)); // 10 toString 
alert(Number(aa)); // 10 toString 
alert(aa == '10'); // true toString 
再看valueOf。 
var bb = { 
i: 10, 
valueOf: function() { 
console.log('valueOf'); 
return this.i; 
} 
} 
alert(bb);// [object Object] 
alert(+bb); // 10 valueOf 
alert(''+bb); // 10 valueOf 
alert(String(bb)); // [object Object] 
alert(Number(bb)); // 10 valueOf 
alert(bb == '10'); // true valueOf


发现有点不同吧？！它没有像上面toString那样统一规整。对于那个[object Object]，我估计是从Object那里继承过来的，我们再去掉它看看。 
Object.prototype.toString = null; 
var cc = { 
i: 10, 
valueOf: function() { 
console.log('valueOf'); 
return this.i; 
} 
} 
alert(cc);// 10 valueOf 
alert(+cc); // 10 valueOf 
alert(''+cc); // 10 valueOf 
alert(String(cc)); // 10 valueOf 
alert(Number(cc)); // 10 valueOf 
alert(cc == '10'); // true valueOf 
```

> 总结：valueOf偏向于运算，toString偏向于显示。
> 
> 1、 在进行强转字符串类型时将优先调用toString方法，强转为数字时优先调用valueOf。
> 
> 2、 在有运算操作符的情况下，valueOf的优先级高于toString。
> 
> 3.如果只重写了toString，对象转换时会无视valueOf的存在来进行转换。但是，如果只重写了valueOf方法，在要转换为字符串的时候会优先考虑valueOf方法。在不能调用toString的情况下，只能让valueOf上阵了。对于那个奇怪的字符串拼接问题，可能是出于操作符上，翻开ECMA262-5 发现都有一个getValue操作。嗯，那么谜底应该是揭开了。重写会加大它们调用的优化高，而在有操作符的情况下，valueOf的优先级本来就比toString的高。

### 五.数组
数组是值的有序集合。每个值叫做元素，每个元素在数组中都有数字位置编号，也就是索引。JS中的数组是弱类型的，数组中可以含有不同类型的元素。数组元素甚至可以是对象或其它数组。


```
var arr = [1, true, null, undefined, {x : 1}, [1, 2, 3]];
```

#### 1.创建数组-字面量

```
var BAT = ['Alibaba', 'Tencent', 'Baidu'];
var students = [{name : 'Bosn', age : 27}, {name : 'Nunnly', age : 3}];
var arr = ['Nunnly', 'is', 'big', 'keng', 'B', 123, true, null];
var arrInArr = [[1, 2], [3, 4, 5]];

var commasArr1 = [1, , 2]; // 1, undefined, 2
var commasArr2 = [,,]; // undefined * 2

size from 0 to 4,294,967,295(2^23  -1 )
```

#### 2.创建数组-new Array

```
var arr = new Array(); 
var arrWithLength = new Array(100); // undefined * 100
var arrLikesLiteral = new Array(true, false, null, 1, 2, "hi");
// 等价于[true, false, null, 1, 2, "hi"];
```

new可以省略

#### 3.数组元素读写

```
var arr = [1, 2, 3, 4, 5];
arr[1]; // 2
arr.length; // 5


arr[5] = 6;
arr.length; // 6


delete arr[0];
arr[0]; // undefined
```

#### 4.数组元素增删（动态的，无需指定大小）

```
var arr = [];
arr[0] = 1;
arr[1] = 2;
arr.push(3);
arr; // [1, 2, 3]

arr[arr.length] = 4; // equal to arr.push(4);
arr; // [1, 2, 3, 4]

arr.unshift(0);
arr; // [0, 1, 2, 3, 4];

delete arr[2];
arr; // [0, 1, undefined, 3, 4]
arr.length; // 5
2 in arr; // false

arr.length -= 1;
arr; // [0, 1, undefined, 3, 4],  4 is removed

arr.pop(); // 3 returned by pop
arr; // [0, 1, undefined], 3 is removed

arr.shift(); // 0 returned by shift
arr; // [1, undefined]
```

> 1.向后插入元素 push()
> 
> 2.向前插入元素 unshift()
> 
> 3.从后移除元素 pop()
> 
> 4.从前移除元素 shift()
> 
> 5.length不固定，可随意更改
> 
> 6.delete只会删除当前位置的值，长度不会变

#### 5.数组迭代

```
var i = 0, n = 10;
var arr = [1, 2, 3, 4, 5];
for (; i < n; i++) {
    console.log(arr[i]); // 1, 2, 3, 4, 5
}

for(i in arr) {
    console.log(arr[i]); // 1, 2, 3, 4, 5
}

Array.prototype.x = 'inherited';

for(i in arr) {
    console.log(arr[i]); // 1, 2, 3, 4, 5, inherited
}

for(i in arr) {
    if (arr.hasOwnProperty(i)) {
        console.log(arr[i]); // 1, 2, 3, 4, 5
    }
}
```

for…in 会受原型影响

#### （2）.二维数组、稀疏数组

```
var arr = [[0, 1], [2, 3], [4, 5]];
var i = 0, j = 0;
var row;
for (; i < arr.length; i++) {
row = arr[i];
console.log(‘row ‘ + i);
for (j = 0; j < row.length; j++) {
console.log(row[j]);
}
}

// result:
// row 0
// 0
// 1
// row 1
// 2
// 3
// row 2
// 4
// 5
```

稀疏数组并不含有从0开始的连续索引。一般length属性值比实际元素个数大。


```
var arr1 = [undefined];
var arr2 = new Array(1);
0 in arr1; // true
0 in arr2; // false
arr1.length = 100;
arr1[99] = 123;
99 in arr1; // true
98 in arr1; // false

var arr = [,,];
0 in arr; // false
```

#### （3）.数组方法

#### 1.Array.prototype.join（将数组转为字符串）

```
var arr = [1, 2, 3];
arr.join(); // "1,2,3"
arr.join("_"); // "1_2_3"

function repeatString(str, n) {
     return new Array(n + 1).join(str);
}
repeatString("a", 3); // "aaa"
repeatString("Hi", 5); // "HiHiHiHiHi"
```

#### 2.Array.prototype.reverse（将数组逆序）

```
var arr = [1, 2, 3];
arr.reverse(); // [3, 2, 1]
arr; // [3, 2, 1]
//原数组被修改
```

#### 3.Array.prototype.sort（排序）

```
var arr = ["a", "d", "c", "b"];
arr.sort(); // ["a", "b", "c", "d"]

arr = [13, 24, 51, 3];
arr.sort(); // [13, 24, 3, 51]
arr; // [13, 24, 3, 51]
//原数组被修改，
```

因为sort是按字母排序的，所以数字排序运用函数


```
arr.sort(function(a, b) {
     return a - b;
}); // [3, 13, 24, 51]
arr = [{age : 25}, {age : 39}, {age : 99}];
arr.sort(function(a, b) {
     return a.age - b.age;
});
arr.forEach(function(item) {
     console.log('age', item.age);
});
// result:
// age 25
// age 39
// age 99
```

#### 4.Array.prototype.concat（数组合并）

```
var arr = [1, 2, 3];
arr.concat(4, 5); // [1, 2, 3, 4, 5]
arr; // [1, 2, 3] 原数组被修改

arr.concat([10, 11], 13); // [1, 2, 3, 10, 11, 13]

arr.concat([1, [2, 3]]); // [1, 2, 3, 1, [2, 3]]
```

#### 5.Array.prototype.slice（返回部分数组）

```
var arr = [1, 2, 3, 4, 5];
arr.slice(1, 3); // [2, 3]
arr.slice(1); // [2, 3, 4, 5]
arr.slice(1, -1); // [2, 3, 4]
arr.slice(-4, -3); // [2]
//原数组未被修改
```

#### 6.Array.prototype.splice（数组拼接）

```
var arr = [1, 2, 3, 4, 5];
arr.splice(2); // returns [3, 4, 5]
arr; // [1, 2];

arr = [1, 2, 3, 4, 5];
arr.splice(2, 2); // returns [3, 4] 第二个参数表示截取几个
arr; // [1, 2, 5];

arr = [1, 2, 3, 4, 5];
arr.splice(1, 1, 'a', 'b'); // returns [2]
arr; // [1, "a", "b", 3, 4, 5]
//原数组被修改
```

#### 7.Array.prototype.forEach（数组遍历）

```
var arr = [1, 2, 3, 4, 5];
arr.forEach(function(x, index, a){
    console.log(x + '|' + index + '|' + (a === arr));
});
// 1|0|true
// 2|1|true
// 3|2|true
// 4|3|true
// 5|4|true
```

#### 8.Array.prototype.map（数组映射）

```
var arr = [1, 2, 3];
arr.map(function(x) {
     return x + 10;
}); // [11, 12, 13]
arr; // [1, 2, 3]
//原数组未被修改
```

#### 9.Array.prototype.filter（数组过滤）

```
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
arr.filter(function(x, index) {
     return index % 3 === 0 || x >= 8;
}); // returns [1, 4, 7, 8, 9, 10]
arr; // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
//原数组未被修改
```

#### 10.Array.prototype.every & some（数组判断）

```
var arr = [1, 2, 3, 4, 5];
arr.every(function(x) {
     return x < 10;
}); // true

arr.every(function(x) {
     return x < 3;
}); // false

var arr = [1, 2, 3, 4, 5];
arr.some(function(x) {
     return x === 3;
}); // true

arr.some(function(x) {
     return x === 100;
}); // false
```

类似逻辑与、逻辑或

#### 11.Array.prototype.reduce&reduceRight

```
var arr = [1, 2, 3];
var sum = arr.reduce(function(x, y) {
     return x + y
}, 0); // 6
arr; //[1, 2, 3]

arr = [3, 9, 6];
var max = arr.reduce(function(x, y) {
     console.log(x + "|" + y);
     return x > y ? x : y;
});
// 3|9
// 9|6
max; // 9
max = arr.reduceRight(function(x, y) {
     console.log(x + "|" + y);
     return x > y ? x : y;
});
// 6|9
// 9|3
max; // 9
```

整个数组聚合成唯一一个结果

#### 12.Array.prototype.indexOf&lastIndexOf （数组检索）

```
var arr = [1, 2, 3, 2, 1];
arr.indexOf(2); // 1
arr.indexOf(99); // -1
arr.indexOf(1, 1); // 4
arr.indexOf(1, -3); // 4
arr.indexOf(2, -1); // -1
arr.lastIndexOf(2); // 3
arr.lastIndexOf(2, -2); // 3
arr.lastIndexOf(2, -3); // 1
```

第一个参数是检索的对象，第二个参数是检索的位置，负数即从后面开始。

#### 13.Array.isArray（判断是否为数组）

```
Array.isArray([]); // true
```

#### （4）.数组 VS. 一般对象
相同

都可以继承
数组是对象，对象不一定是数组
都可以当做对象添加删除属性

不同

数组自动更新length
按索引访问数组常常比访问一般对象属性明显迅速。
数组对象继承Array.prototype上的大量数组操作方法

#### (5).字符串和数组

```
var str = "hello world";
str.charAt(0); // "h"
str[1]; // e

Array.prototype.join.call(str, "_");
// "h_e_l_l_o_ _w_o_r_l_d"
```

> call()和apply()方法
> 
> 1、方法定义
> 
> call方法:
> 
> 
> ```
> 语法：call([thisObj[,arg1[, arg2[, [,.argN]]]]])
> ```
> 
> 
> 定义：调用一个对象的一个方法，以另一个对象替换当前对象。
> 
> 说明：
> call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。
> 如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。
> 
> apply方法：
> 
> 
> ```
> 语法：apply([thisObj[,argArray]])
> ```
> 
> 
> 定义：应用某一对象的一个方法，用另一个对象替换当前对象。
> 
> 说明：
> 如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。
> 如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。
> 
> 2、常用实例
> 
> a、
> 
> 
> ```
> function add(a,b)  
> {  
>     alert(a+b);  
> }  
> function sub(a,b)  
> {  
>     alert(a-b);  
> }  
> 
> add.call(sub,3,1);
> ```
> 
> 这个例子中的意思就是用 add 来替换 sub，add.call(sub,3,1) == add(3,1) ，所以运行结果为：alert(4); // 注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。
> 
> b、
> 
> 
> ```
> function Animal(){    
>     this.name = "Animal";    
>     this.showName = function(){    
>         alert(this.name);    
>     }    
> }    
> 
> function Cat(){    
>     this.name = "Cat";    
> }    
> 
> var animal = new Animal();    
> var cat = new Cat();    
> 
> //通过call或apply方法，将原本属于Animal对象的showName()方法交给对象cat来使用了。    
> //输入结果为"Cat"    
> animal.showName.call(cat,",");    
> //animal.showName.apply(cat,[]);
> ```
>   
> call 的意思是把 animal 的方法放到cat上执行，原来cat是没有showName() 方法，现在是把animal 的showName()方法放到 cat上来执行，所以this.name 应该是 Cat
> 
> c、实现继承
> 
> 
> ```
> function Animal(name){      
>     this.name = name;      
>     this.showName = function(){      
>         alert(this.name);      
>     }      
> }      
> 
> function Cat(name){    
>     Animal.call(this, name);    
> }      
> 
> var cat = new Cat("Black Cat");     
> cat.showName();
> ```
> 
> Animal.call(this) 的意思就是使用 Animal对象代替this对象，那么 Cat中不就有Animal的所有属性和方法了吗，Cat对象就能够直接调用Animal的方法以及属性了.
> 
> d、多重继承
> 
> 
> ```
> function Class10()  
> {  
>     this.showSub = function(a,b)  
>     {  
>         alert(a-b);  
>     }  
> }  
> 
> function Class11()  
> {  
>     this.showAdd = function(a,b)  
>     {  
>         alert(a+b);  
>     }  
> }  
> 
> function Class2()  
> {  
>     Class10.call(this);  
>     Class11.call(this);  
> }
> ```
> 
> 很简单，使用两个 call 就实现多重继承了
> 
> 说了call ，当然还有 apply，这两个方法基本上是一个意思，区别在于 call 的第二个参数可以是任意类型，而apply的第二个参数必须是数组，也可以是arguments
> 还有 callee，caller..

### 六.函数
函数是一块JavaScript代码，被定义一次，但可执行和调用多次。
JS中的函数也是对象，所以JS函数可以像其它对象那样操作和传递 所以我们也常叫JS中的函数为函数对象。

#### 不同的调用方式
直接调用
foo();

对象方法
o.method();

构造器
new Foo();

call/apply/bind
func.call(o);

#### (2).函数声明和函数表达式
#### 1.函数声明

```
function add (a, b) { a = +a;
b = +b;
if (isNaN(a) || isNaN(b)) { return;
}
return a + b;
}
```

函数表达式

```
// function variable
var add = function (a, b) {
// do sth
};

// IEF(Immediately Executed Function)
(function() {
// do sth
})();

// first-class function 
return function() {
// do sth
};

// NFE (Named Function Expression)
//命名函数表达式（有名字的函数表达式） 
var add = function foo (a, b) {
// do sth

};
```

#### 2.变量&函数的声明前置

```
//函数声明会前置，即
//function add(a,b) 
var num = add(1, 2);
console.log(num);  //result:3

function add (a, b) {
    a = +a;
    b = +b;
    if (isNaN(a) || isNaN(b)) 
        { return;
    }
    return a + b;
}

//函数表达式中，函数不会前置，可是变量会
//var add 但是它是undefined，没有赋值，只是声明
var num = add(1, 2);
console.log(num);

vaar add = function (a, b) {
    a = +a;
    b = +b;
    if (isNaN(a) || isNaN(b)) 
        { return;
    }
    return a + b;
}
```

#### 3.命名函数表达式

```
var func = function nfe() {};
alert(func === nfe);
```

> IE6~8中，会弹出false窗口，即两个不相等，可是外部可以访问nfe;
> 
> IE9+中，nfe undefined。即外部不可调用nfe。
> 
> 
> ```
> // 递归调用
> var func = function nfe() {/** do sth.**/ nfe();}
> ```

#### 4.Function 构造器

```
var func = new Function('a', 'b', 'console.log(a + b);');
func(1, 2);    // 3

var func = Function('a', 'b', 'console.log(a + b);'); func(1, 2);    // 3
//CASE 1
Function('var localVal = "local";
console.log(localVal);')();
console.log(typeof localVal);
// result: local, undefined
```

localVal仍为局部变量

/
```
/ CASE 2
var globalVal = 'global';
(function() {
    var localVal = 'local';
    Function('console.log(typeof localVal, typeof globalVal);')();
})();
// result: undefined, string
```

local不可访问，全局变量global可以访问



#### （2）.this

#### 1.全局的this（浏览器）

```
console.log(this.document === document); // true
console.log(this === window); // true this.a = 37;
console.log(window.a); // 37
```

#### 2.一般函数的this（浏览器）

```
function f1(){ 
    return this;
}

f1() === window; // true, global object

//严格模式的this指向undefined（全局）
function f2(){
    "use strict"; // see strict mode return this;
}

f2() === undefined; // true
```

#### 3.作为对象方法的函数的this

```
var o = 
{ prop: 
37,
    f: function() 
    { return 
        this.prop;
    }
};

console.log(o.f()); // logs 37
var o = {prop: 37};

function independent() 
{ return this.prop;
}

o.f = independent;

console.log(o.f()); // logs 37
```

#### 4.对象原型链上的this

```
var o = {f:function(){ return this.a + this.b; }}; 
var p = Object.create(o);
p.a = 1;
p.b = 4;

console.log(p.f()); // 5
```

#### 5.get/set方法与this

```
function modulus(){
    return Math.sqrt(this.re * this.re + this.im * this.im);
}

var o = 
    { re: 1,
    im: -1,
    get phase(){
        return Math.atan2(this.im, this.re);
    }
};


Object.defineProperty(o, 'modulus', {
get: modulus, enumerable:true, configurable:true});

console.log(o.phase, o.modulus); // logs -0.78 1.4142
```

get,set的this指向它本身的函数

#### 6.构造器中的this

```
function MyClass(){
this.a = 37;

}

var o = new    MyClass(); 
console.log(o.a); // 37


function C2(){ 
    this.a = 37; 
    return {a : 38};
}

o = new    C2(); 
console.log(o.a); // 38
```

#### 7.call/apply方法与this

```
function add(c, d){
return this.a + this.b + c + d;

}

var o = {a:1, b:3};

add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16

add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34 

function bar() {
    console.log(Object.prototype.toString.call(this));

}

bar.call(7); // "[object Number]"
```

#### 8.bind方法与this（IE9+才有)

```
function f(){ 
    return this.a;
}

var g = f.bind({a : "test"}); 
console.log(g()); // test

var o = {a : 37, f : f, g : g};
console.log(o.f(), o.g()); // 37, test
```

#### （4）.函数属性和arguments

```
function foo(x, y, z) {
‘use strict’;
arguments.length; // 2
arguments[0]; // 1

arguments[0] = 10;
x; // change to 10; //绑定关系，严格模式下仍然是1

arguments[2] = 100;
z; // still undefined !! //未传参数失去绑定关系

arguments.callee === foo; // true //严格模式下不能使用
}
```

#### 1.apply/call方法（浏览器）

```
function foo(x, y) 
{ 
    console.log(x, y, this);
}

foo.call(100, 1, 2); // 1, 2, Number(100)
foo.apply(true, [3, 4]); // 3, 4, Boolean(true) 
foo.apply(null); // undefined, undefined, window 
foo.apply(undefined); // undefined, undefined, window
function foo(x, y) {
    'use strict'; 
    console.log(x, y, this);
}

foo.apply(null); //undefined,undefined,null 

foo.apply(undefined); // undefined, undefined, undefined
```

#### 2.bind方法

```
this.x = 9;
var module = { 
    x: 81,
    getX: function() { return this.x; }
};

module.getX(); // 81

var getX = module.getX; 
getX(); // 9 指向全局变量

var boundGetX = getX.bind(module); 
boundGetX(); // 81 将这个this指向参数的module
```

#### 3.bind与currying（函数颗粒化，把一个函数拆成多个单元）
f
```
unction add(a, b, c) {
    return a + b + c;
}

var func = add.bind(undefined, 100); 
func(1, 2); // 103 
//很多时候，我们不需要传入那么多参数，这里的100，固定赋值给a

var func2 = func.bind(undefined, 200); func2(10); // 310
function getConfig(colors, size, otherOptions) {
    console.log(colors, size, otherOptions);
}

var defaultConfig = getConfig.bind(null, "#CC0000", "1024 * 768"); 
defaultConfig("123"); // #CC0000 1024 * 768 123
defaultConfig("456"); // #CC0000 1024 * 768 456
```

#### 4.bind 和 new

```
function foo() {
    this.b = 100; 
    return this.a;
}

var func = foo.bind({a:1}); 

func(); // 1
new func(); // {b : 100}
```

直接调用foo()的话，this指向全局对象，这里面相对于创建了一个全局变量b，并且赋值为100，返回的是全局变量的a属性

用bind的方法传入一个参数，一个字面量，只有一个属性a，直接调用的话，则this会指向bind的这样的一个参数。

用new的话，return除了是一个对象，否则会以this作为返回值，并且this会被初始化为一个默认的空对象，这个对象的原型是foo.prototype。虽然前面bind了，可是new了之后，会忽略bind，this指向么有bind所指向的。意思是如果用new调用的话，bind的作用会被忽略掉。

#### （5）.理解闭包

当一个函数被调用完毕之后，这个局部变量就可以被释放掉了

闭包，以自己的话理解就是，函数被调用完后，局部变量没有被释放。执行上下文仍在栈中，没有出栈。因为下一个执行上下文需要用到。

**常见错误之循环闭包**

```
document.body.innerHTML = "<div  id=div1>aaa</div>"+ "<div id=div2>bbb</div><div id=div3>ccc</div>"; 

for (var i = 1; i < 4; i++) {
document.getElementById('div' + i). addEventListener('click', function()
 { alert(i); // all are 4!
    });
}
document.body.innerHTML = "<div  id=div1>aaa</div>"+ "<div id=div2>bbb</div><div id=div3>ccc</div>"; 

for (var i = 1; i < 4; i++) {
!function(i) 
        { 
            document.getElementById('div' + i).
            addEventListener('click', function() { alert(i); // 1, 2, 3
        });
    }(i);
}
//将每次遍历的1，2，3的值，传到匿名函数（下面那个i），然后在这个匿名函数里面，再用到这个参数i
```

**闭包封装**

```
(function() {
    var _userId = 23492; 
    var _typeId = 'item'; 
    var export = {};

    function converter(userId) { 
        return +userId;
    }

    export.getUserId = function() { 
        return converter(_userId);
    }

    export.getTypeId = function() { 
        return _typeId;
    }

    window.export = export;

}());

export.getUserId(); // 23492 
export.getTypeId();    // item

export._userId; // undefined 
export._typeId; // undefined 
export.converter; // undefined
```

函数有函数自己的作用域，里面定义的变量，外部是无法访问的。

我们在函数中定义变量，外部无法用，再用window.export=export这样的方式，把我们最终想输出的对象输出去

外部只能通过export这个对象提供的方法来访问到里面的具体的函数的变量

#### （6）.作用域

```
var a = 10; 全局
(function() {
var b = 20;函数
})();

console.log(a);    // 10
console.log(b);    // error, b in not defined

for (var item in {a : 1, b : 2}) { 
    console.log(item);

}

console.log(item);    // item still in scope

eval("var a = 1;"); //eval
```

作用域链

```
function outer2() {
    var local2 = 1; 
    function outer1() {
        var local1 = 1;
        // visit local1, local2 or global3 作用域链从初开始向上找创建该作用域的函数
    }
    outer1();
}
var global3 = 1;
outer2();

function outer() { 
    var i = 1;
    var func = new Function("console.log(typeof i);"); 
    func();    // undefined}  //Function中的变量外部无法访问
outer();
```

利用函数作用域封装

```
(function() {
    // do sth here 
    var a, b;
})();

!function() {
    // do sth here 
    var a, b;
}();
```

写成函数表达式，为了防止变量被提前声明。留下一堆括号，会报语法错误

#### （7）.ES3执行上下文
图文详情参见《js深入了解闭包和原型》

JS解释器如何找到我们定义的函数和变量？

变量对象(Variable Object, 缩写为VO)是一个抽象 概念中的“对象”，它用于存储执行上下文中的：

1. 变量
1. 函数声明
1. 函数参数

```
var a = 10; 
function test(x) {
    var b = 20;
}
test(30);

VO(globalContext) = { 
    a : 10,
    test : <ref to function>
};
VO(test functionContext) = { 
    x : 30,
    b: 20
};
```

1.变量初始化阶段（对于函数来说）
> **VO按照如下顺序填充:**
> 
> 1. 函数参数 (若未传⼊入，初始化该参数值为undefined)
> 1. 函数声明 (若发⽣生命名冲突，会覆盖)
> 1. 变量声明 (初始化变量值为undefined，若发⽣生命名冲突，会忽略。)


```
function test(a, b) {
    var c = 10; function d() {}
    var e = function _e() {}; (function x() {});
    b = 20;
}
test(10);

AO(test) = { 
    a: 10,
    b: undefined, 
    c: undefined,
    d: <ref to func "d"> 
    e: undefined
};
//函数表达式不会影响VO
```

#### 2.代码执行阶段

```
function test(a, b) {
    var c = 10; function d() {}
    var e = function _e() {}; (function x() {});
    b = 20;
}
test(10);

AO(test) = { 
    a: 10,
    b: 20,
    c: 10,
    d: <reference to FunctionDeclaration "d"> 
    e: function _e() {};
};
测试
alert(x);    // function

var x = 10; 
alert(x);    // 10
x = 20;

function x() {} 
alert(x);    // 20

if (true) { 
    var a = 1;
} else {
    var b = true;
}

alert(a);    // 1
alert(b);    // undefined
```

首先，vo对象里面，有一个x，值是一个函数对象（函数声明前置），有一个a和b，值是undefined。var x=10：因为变量名和函数名冲突，忽略

然后进入代码执行阶段，var x=10,赋值成功，alert为10；因为b被前置处理，所以alert b不会报错，undefined

### 七.oop（面向对象程序设计）
#### 1.继承

#### 2.原型链

#### 3.prototype

#### 4.instanceof

以上《js深入了解闭包和原型》

#### 5.实现继承的方法


```
Student.prototype = Object.create(Person.prototype)
```

创建一个空对象，并将其原型指向person.prototype(ES5之后才可以使用）


```
if(!Object.create){
    Object.create = function(proto){
        function F(){}
        F.prototype = proto;
        return new F;
    };
}//ES5之前使用方法
```

#### 6.模拟重载
重载的定义是指函数的方法名相同，但参数不同。比如我写两个关于js的同名函数：


```
<script type = "text/javascript" >
    function add(a,b){
        var sum = a+b;
        alert (sum);
    }
    add(2,3);

    //第二个函数
    function add(c,d,e){
        var sum2 = c+d+e;
        alert (sum2);
    }
    add(2,3,4);

</script>
```

结果为NaN 9 ，这就说明后面的函数把前一个同名函数覆盖掉了，从而可以得出js函数不存在重载，永远调用最后一个方法。

虽说JS函数不存在重载，但是我们可以实现js里面的模拟重载，接下来见证奇迹的时刻到了。要想实现js模拟重载必先回用到 arguments.用来存放js参数。实现方法如下：


```
function add1(){
    //比如传递的是两个参数
    if(arguments.length == 2){
        return arguments[0] +arguments[1];
    }else if(arguments.length ==3){
        return arguments[0]+arguments[1]+arguments[2];
    }else if(arguments.length == 4){
        return arguments[0]+arguments[1]+arguments[2]+arguments[3];
    }else{
        return 0 ;
    }
}

    //调用
    alert(add1(1,2));
    alert(add2(1,2,3));
    alert(add3(1,2,3,4));
    alert(add4(1,2,3,4,5));

    结果 3 6 10 0
```

#### 7.调用子类的方法（子掉父）

```
function Person(name){
  this.name=name;
}

function Student(name,className){
  this.className=className;
  Person.call(this,name);//调用别的构造器   
}

var mike=new Student('mike','Network3');

//子类调用已被覆盖的父类的方法
Person.prototype.init=function{
...
}
Student.prototype.inti=function{
  Person.prototype.init.apply(this,arguments);    
}
```

#### 8.链式调用

```
function ClassManager(){} //先定义一个构造器
ClassManager.prototype.addClass = function(str){
    console.log('Class + str + 'added');
    return this;
};

 //然后在ClassMannager构造器上增加addclass方法,需要注意一点，最后return this，this指向classmanager的实例

var manager = new ClassManager();
manager.addClass('classA').addClass('classB').addClass('classC');
//Class:class A added
//Class:class B added
//Class:class C added
```

#### 9.抽象类**
9.模块化
所谓模块化就是将需要用到的方法封装成为一个模块，哪里需要直接调用即可，比如你需要计算1+1和2+2，那么你只需要调用a+b的方法即可，无需反复重写，但是由于js并不支持类的继承，因此如何封装和调用方法就成为js模块化的重要思路。

最简单的方式


```
function sum(a,b){
        alert(a+b);
    }
function sub(a,b){
        alert(a-b);
    }
    sum(1,1);
    sum(2,2);
    sub(11,1);
```

上面的sum（），sub（）分别都可以算作是一个模块，需要用的时候直接调用即可，但是这种做法的缺点很明显：”污染”了全局变量，无法保证不与其他模块发生变量名冲突，而且模块成员之间看不出直接关系。比如a，b是全局变量同时方法中需要改变a，b值的时候。

因此大多时候我们都会将其采用对象的写法：


```
var Sum=new Object({
        a:1,
        b:2,
        sum:function(){
            alert(this.a-this.b);
        }
    });
    Sum.sum();
```

上面的函数sum()，封装在Sum对象里。使用的时候，就是调用这个对象的属性。

Sum.sum();

但是，这样的写法会暴露所有模块成员，内部状态可以被外部改写,比如：

sum.a=2;

因此，在这个的基础上，我们可以采用立即执行函数写法：


```
var Sum=(function(){
       var a=1;
       var b=2;
       var sum=function(){
           alert(a+b);
       };
       var sub=function(){
           alert(a-b);
       };
       return {
           sum:sum,
           sub:sub
       }
   })();
   alert(Sum.a);
   Sum.sum();
```

第一个输出的Sum.a是undefind，外部是无法获取我们模块内部参数的，但是由于方法我们返回成为一个对象，因此是可以由外部调用的。

同时该模块还可以为其添加新的方法


```
Sum=(function () {

        m3 = function () {
            alert("开始计算");
        };
        return {m3:m3}
    })(Sum);
        Sum.m3();
```

但是注意，这里不能直接调用Sum中的变量a，b，如果需要使用这些参数还需要重新赋予，关于解决方式大家有好的办法欢迎回复。

另外为了避免执行顺序等干扰造成程序报错，在添加方法时也可以写成


```
Sum=(function () {

    m3 = function () {
        alert("im");
    };
    return {m3:m3}
})(window.Sum || {});
```

这样即使是空对象也可以添加成功。

### 八.正则与模式匹配
#### 1.什么是正则？
在常见的字符串检索或替换中，我们需要提供一种模式表示检索或替换的规则。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串

#### 2.正则基础


#### 3.范围符号


#### 4.特殊字符转义

```
/\^abc/.test('^abc');
```

#### 5.分组


#### 6.重复


#### 7.三个Flag
global

ignoreCase

multiline


```
/abc/gim.test("ABC"); //true
RegExp("abc","mgi");
```

#### 8.RegExp 对象属性
global

ignoreCase

multiline

source


```
/abc/g.global //true
/abc/g.ignoreCase //false
/abc/g.multiline //false
/abc/g.source //"abc"
```

#### 9.RegExp 对象方法
complie exec test toString


```
/abc/.exec("abcdef"); //"abc"
/abc/.test("abcde"); //true
/abc/.toString(); //"/abc/"

var reg = /abc/;
reg.compile("def");
reg.test("def"); //true
```

#### 10.string类型与正则相关的方法
String.prototype.search


```
"abcabcdef".search(/(abc)\1); //0
```

String.prototype.replace


```
"aabbbbcc".repalce(/b+?/,"1"); //aa1bbbcc
```

String.prototype.match


```
"aabbbbcc".match(/b+/); //["bbbb"]

"aabbbbccbbaa".match(/b+/g); //["bbbb","bb"]
```

String.prototype.split


```
"aabbbbccbbaa".split(/b+/); //["aa","cc","aa"]
```

