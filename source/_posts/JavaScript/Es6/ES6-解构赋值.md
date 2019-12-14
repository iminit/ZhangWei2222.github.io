---
title: ES6-解构赋值
date: 2019-06-11 19:29:37
categories:
- JavaScript
tags:
- Es6
comments: false
---

## 前言
看到大家有这样的写法，很方便，但是为什么可以这样写呢？
```javascript
import {常量1,常量2,...} from 路径 
const {MWP} = M;
```

## 什么是解构赋值
Es6允许按照一定的模式，从数组和对象中提取值，对变量进行赋值——解构

```javascript
比如之前必须：
let a = 1;
let b = 2;

现在可以
let [a, b] = [1, 2];
```
## 数组的解构赋值-注意点
#### 1、左右两边格式需要对等，数量可以不对等；不对等将赋值undefined
```javascript
let [b, a] = [1];
//b=1 a=undefined
```

#### 2、左右两边为其他类型会报错；字符串例外
```javascript
let [a] = 1;
let [a] = false;
let [a] = NaN;
let [a] = undefined;
let [a] = null;
let [a] = {};
以上都会报错

let [a] = '121321'  a = '1' //取字符串第一个下标的值
```

#### 3、Set结构也可以解构赋值
```javascript
let [x,y,z] = new Set([1,2,3])  //x = 1, y = 2, z = 3
```

#### 4、允许指定默认值
```javascript
let [a = 3] = []        // a:3
```

#### 5、undefined 与 null
如果需要默认值生效，则应对等的值为undefined的时候才会采用默认值，否则还是使用赋值
```javascript
let [a = 3] = [undefined]   // a:3
let [a = 3] = [null]        // a:null
```

## 对象的解构赋值-注意点
**非常注意：数组的解构赋值需要等号两边位置一致才能正确赋值；而对象的解构赋值需要等号两边的变量和属性同名一致即能正确赋值，否则undefined**
```javascript
let {a,b} = {a:'3',c:'d'}
//a: 3 b: undefined
```

#### 1、可以将现有对象的方法赋值给一个新变量
```javascript
let {sin,cos} = Math
```

#### 2、注意左边的变量名不能与右边的对象属性名保持一致的情况
必须这样写
	let {a:b} = {a: 'sss'} // b: 'sss'
	

#### 3、允许嵌套赋值
```javascript
第一种：
let obj = {
    p:[
        'Hello',
        {y:'world'}
    ]
}

let {p:[x,{y}]} = obj // x: Hello, y: world
这边的p只是属性不是变量，如果p想赋值可以写成：
let {p,:[x,{y}]} = obj

第二种：
const a = {
    loc: {
        t :1,
        b :{
            c:1,
            d:2    
        }
    }
}
let {loc:{t,b:{c,d}}} = a
或者
let {loc,loc:{t,b,b:{c,d}}} = a
```
##### 如果子对象所在的父属性不存在，则会报错，如下：
```javascript
let {foo:{bar}} = {baz:'baz'} //报错
```

#### 4、允许指定默认值
```javascript
let {x = 3} = {} //x:3
注意：不能将已声明的变量用于解构赋值，因为已经是一个代码块
```

## 字符串的解构赋值-注意点
**如果赋值的对象是数组，字符串将被分割为数组的格式一一对应赋值**
```javascript
let [a,b] = 'ha' // a = h , b = a
let {length:len} = '12121' // len = 5
```

## 数值和布尔值的解构赋值-注意点
如果等号右边是 数值和布尔值，会把他们转化成对象（没有赋值），如果是undefined和null会报错(无法转成对象)
```javascript
let {a: b} = 123; b === Number.prototype.a;
let {a: b} = true; b === Boolean.prototype.a;
let {a: b}=undefined;  //报错
let {a: b}=null;   //报错
```


## 函数参数的解构赋值-注意点
```javascript
注意以下两种写法(区别：左边传值，右边赋值；传值会覆盖赋值)
function m({x=3, y=4} = {}){
    return [x,y]
}
m({x:33,y:8})     // [33,8]
m({x:32})         // [32,4]
m({})             // [3,4]
m()               // [3,4]

function m({x,y} = {x=0,y=0}){
    return [x,y]
}
m({x:33,y:8})     // [33,8] 
//代替右边的 x:0, y:0 所以是传值 33  8

m({x:32})         // [32,undefined]
//因为传值替代右边的赋值，但是只有x没有y
//所以y是取左边y的默认值，因为你没有赋值 为undefined

m({})             // [undefined,undefined] 
// 取左边x,y的默认值，因为没有赋值 为undefined

m()               // [0,0]
// 没有传值，使用本身的赋值 都是0
```

## 圆括号问题
#### 1、声明语句不能使用
```javascript
var [(a)]=[1];
```

#### 2、函数参数不能使用
```javascript
function f([(z)]) = { return z; }
```

#### 3、不能讲整个模式或嵌套模式中的一层放在圆括号中
```javascript
({p: a})={p: 42}
([a]) = [5]
```

### 正确：把圆括号放在非模式部分
```javascript
[(b)] = [3];
({p: (d)} = {})
```

## 常用情景
#### 1、变换变量的值
```javascript
[x,y]=[y,x]
```

#### 2、从函数中返回多个值
```javascript
// 返回一个数组
function f(){
    return [1,2,3];
}
var [a,b,c] = f(); 

// 返回一个对象
function f1(){
    ruturn {
        foo: 1,
        bar: 2
    }
}
var {foo, bar} = f1();
```

#### 3、函数参数的定义
```javascript
function f([x,y,z]){...}
f({1,2,3})

function f([x,y,z]){...}
f({x:1,z:3,y:2})
```

#### 4、提取JSON数据
```javascript
var jsonData= {
    id: 42,
    status: 'ok',
}
let {id,status: isok} = jsonData;
consoloe.log(id, isok); // 42 "OK"
```

#### 5、函数参数的默认值
```javascript
function f(a=1, b=2) {}
```

#### 6、遍历Map解构
```javascript
var map = new Map() ;

map.set('first', 'hello');
map.set('sec', 'world');

for(let [key, value] of map){
    // 从循环的数值中依次赋值key和value
    console.log(key + "is" + value)
    // first is hello 
    // sec is world 
}
```

#### 7、输入模块的制定方法
```javascript
const { SourceMapConsumer, SourceNode} = require("source-map")
```