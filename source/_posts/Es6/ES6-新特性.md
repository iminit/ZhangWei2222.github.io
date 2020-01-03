---
title:  ES6-新特性
date: 2019-03-14 15:32:37
categories:
- Es6
comments: false
---

## 1、let const

引入块级作用域的概念；ES6前，Js 没有块级作用域 {} 的概念

#### let 与 var 与 const 区别

- **let、const** 
  - 都是 ES6 新提出的命令
  - 都是块级作用域，只在命令所在的代码块内有效
  - 不会变量提升
- **var** 
  - 不是块级作用域
  - 会变量提升：在函数中央声明变量，实际上整个函数域都会有效，会造成 变量的混淆等情况
- **const**
  - 用来定义常量，使用时必须赋值
  - 基本类型不能修改，引用类型可以修改：赋值的是一个指针，一个地址；改变的时候地址并没有变



## 2、import、export

import导入模块、export导出模块



## 3、箭头函数 arrow functions 

```js
let func = （num) => num;
let func = () => num;
let sum = (num1,num2) => num1 + num2;
[1,2,3].map(x => x * x);
```

- 本身没有this，不能用call()、apply()、bind()这些方法去改变this的指向

- this是继承 父执行上下文 中的this，在定义函数的时候绑定，而不是在执行函数的时候绑定

- 不能作为构造函数，不能使用 new，不然会报错（箭头函数没有实例）

- 没有 arguments，需要用 rest参数 接收

	```js
  let B = (b)=>{
     console.log(arguments);
  }
  B(2,92,32,32);   // Uncaught ReferenceError: arguments is not defined
  
  let C = (...c) => {
     console.log(c);
  }
  C(3,82,32,11323);  // [3, 82, 32, 11323]
  ```

- 没有原型属性

	```js
    var a = ()=>{
      return 1;
    }
    function b(){
      return 2;
    }
    
    console.log(a.prototype);  // undefined
    console.log(b.prototype);   // {constructor: ƒ}
  ```
  
- 返回对象时，要加一个小括号

    ```js
    var func = () => ({ foo: 1 }); //正确
    var func = () => { foo: 1 };   //错误
    ```



## 4、模板字符串 template string 

- 取变量

```js
//` ${}` 反引号 + $ + 中括号变量

var name = "liyang";
console.log(`I am ${name}`)
```

- 多行字符串拼接

	es5 var msg = "Hi \ man!"; 
	es6 const template = `<div> <span>hello world</span> </div>`;



## 5、函数默认参数 default 

```js
// ES5 给函数定义参数默认值 
function foo(num) { 　　
​	num = num || 200; 　　
​	return num; 
}   

// ES6 在 函数参数列表中，赋值，作默认值
function foo(num = 200) { 　　
​	return num; 
} 
```



## 6、rest参数

获取函数的多余参数，放在数组里

```js
function foo(x, y, ...rest) { 　　
  return ((x + y) * rest.length); 
} 
foo(1, 2, 'hello', true, 7); // 9 
```



##  7、展开运算符 Spread Operator

- 组装数组

```js
let color = ['red', 'yellow']; 
let colorful = [...color, 'green', 'blue']; 
console.log(colorful); // ["red", "yellow", "green", "blue"]
```

- 获取数组除了某几项的其他项

```js
let num = [1, 3, 5, 7, 9]; 
let [first, second, ...rest] = num; 
console.log(rest); // [5, 7, 9]
```

​    

## 8、class、extends、super

提供 Class（类） 的概念，一个语法糖，作为对象的模块。使得生成实例 更像面向对象的语法

每个类里面都有一个 construtor方法（构造方法，即使没写，也会默认生成一个），this 指向实例对象

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

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this



## 9、Promise

- 表示一个异步操作的最终状态（成功或失败），以及其返回的值
- Promise 是一个构造函数，自己身上有all、reject、resolve等方法，原型上有then、catch等方法



我们用Promise的时候一般是包在一个函数中，在需要的时候去运行这个函数

```js
function runAsync(){     
  var p = new Promise(function(resolve, reject){         
    //做一些异步操作         
    setTimeout(function(){             
      console.log('执行完成');             
      resolve('随便什么数据');         
    }, 2000);     
  });     
  return p;             
} 
runAsync() 
```

**解析：**

> 构造函数传入两个参数：resolve，reject，分别表示 异步操作执行成功后的回调函数 和 异步操作执行失败后的回调函数
>
> **resolve **将 Promise 的状态置为**fullfiled**，**reject** 将 Promise 的状态置为**rejected**（**pending 初始状态**，既不是成功，也不是失败状态)
>
> 在runAsync() 的返回上直接调用 then 方法，then 接收两个回掉函数作为参数
>
> - promise 对象的状态变为 resolved 时调用
> - promise 对象的状态变为 reject  时调用 （可选）
>
> 还可以调用 catch方法，用来指定 reject 的回调

[原生js实现promise](https://blog.csdn.net/weixin_41440282/article/details/81152645)



#### 怎么调用一个函数，让它实现promise方法

```js
function myRequest() { 
  //内容 
} 
myRequest({url:'11',data:{'a':'1'},method:'get'}).then().catch()  

function myRequest(obj) { 
  return new Promise((resolve,reject)=>{ 
    $.ajax({ 
      url: obj.url, 
      type: obj.type, 
      data: obj.data || '', 
      dataType: 'json', 
      success: function(data){ 
        resolve(data) 
      }, 
      error:function(error){ 
        reject(error) 
      } 
    }) 
  }) 
} 
```



#### all方法

- 提供了**并行执行异步操作**的能力，并且在**所有异步操作 执行完后 才执行回调**
- all 会把**所有异步操作的结果**放进**一个数组**中传给 then



#### race方法

- 当参数的**某个方法执行完了**，就把**异步操作的结果**传给 then，**不影响**其他异步操作的执行



#### async/await

- es7提出的语法糖，Await和Async其实就是**promise的封装**
- **async 的用法**
- **作为一个关键字放到函数前面**，用于表示函数是一个异步函数，因此该函数的执行不会阻塞后面代码的执行
- **async 函数内部的实现原理**
  - 如果async 函数中有返回一个值 ,当调用该函数时，**内部会调用 Promise.resolve() 方法把它转化成一个promise 对象作为返回**，可以使用 **then方法**  获取到promise 返回值
  - 如果函数**内部抛出错误**，就会**调用 Promise.reject() 返回一个 promise 对象**，可以使用 **catch 方法**  获取到promise 返回值

- **await 的用法** 
  - await 关键字只能放到 async 函数里面；**await 后面是一个 promise 对象**，如果不是的话，也会被转换成一个 立即 resolve 的 Promise
  - 当代码**执行到 await 的时候，不再往下执行**，执行完 await 才执行下面的，这样写异步代码就像写同步代码一样了。（再也没有回调地狱了。为了实现某些逻辑经常会写出层层嵌套回调函数，如果嵌套过多，会极大影响代码可读性和逻辑，这种情况也被成为回调地狱）

```js
async function  f() {
    return await 1
};
f().then( (v) => console.log(v)) // 1

function fetchUser() { 
    return new Promise((resolve, reject) => {
        fetch('https://api.github.com/users/superman66')
        .then((data) => {
            resolve(data.json());
        }, (error) => {
            reject(error);
        })
    });
}
async function getUserByAsync(){
     let user = await fetchUser();
     return user;
 }
getUserByAsync()
.then(v => console.log(v));
```


