---
title:  严格模式(use strict)
date: 2019-06-12 15:15:37
categories:
- JavaScript
tags:
comments: false
---

## 支持的浏览器
- IE10+ 
- Firfox4+ 
- Chrome13+ 
- Safari5+ 
- Opera12+

## 声明
在脚本或者函数的头部加'use strict'

## 好处
- 消除js语法不合理、不严谨的地方，减少怪异行为
- 保证代码运行的安全
- 提高编译器效率，增加运行速度
- 为未来版本的js做铺垫

## 限制
- 不允许使用未声明的变量
- 不允许删除变量、函数、不允许删除的属性
- 不允许变量重名
- 不允许函数参数重名
- 不允许使用八进制
- 不允许使用转义字符
- 不允许对只读属性赋值
- 不允许对一个使用getter方法读取的属性赋值
- 不能使用'eval'字符串
    - var eval = 3.14;         // 报错
- 在作用域 eval() 创建的变量不能被调用：
    - eval ("var x = 2");
    - alert (x);               // 报错
- 不能使用'arguments'字符串
    - var arguments = 3.14;         // 报错
- 不允许使用with语句
- 增加了保留字（比如protected、static和interface）

#### 注意：
1、在非严格模式中,修改命名参数的值也会反应到arguments对象中,而严格模式坐下,这两个值是完全独立的

```javascript
// 修改命名参数的值
// 非严格模式:修改会反应到arguments中
// 严格模式中不会反应到arguments中
function sum(num,num2){
  'use strict'
num=3
  console.log(arguments[0],num2)// 严格模式下输出为1,2 非严格模式下输出为3,2
}
sum(1,2)
```

2、禁止this指向全局对象

```javascript
function f(){ 
    "use strict";
    return !this;
} 
// 返回true，因为严格模式下，this的值为undefined，所以"!this"为true
```

使用构造函数时，如果忘了加new，this不再指向全局对象，而是报错。


```javascript
function f(){
    "use strict";
    this.a = 1;
};
f();// 报错，this未定义
```

3、在严格模式中,可以在eval()中声明变量和函数,但这些边行或者函数只能在被求值的特殊作用域中有效,随后就将被销毁,下面这段代码执行就是没问题的


```javascript
'use strict'
var result=eval('x=1,y=13;x+y')
alert(result)
```

在这里,eval中声明了变量x和y,然后将他们加在一起,反悔了他们的和,于是result变量的值就是21,即x+y的结果,在调用alert时,尽管x和y已经不存在了,result变量的值还是有效的