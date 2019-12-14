---
title: Ajax
date: 2019-03-07 12:32:37
categories:
- JavaScript
tags:
- Ajax
comments: false
---

## 前言

Ajax = 异步 JavaScript + XML

**技术核心**： XMLHttpRequest 对象

**请求过程**：创建XMLHttpRequest、连接服务器、发送请求、服务器做出相应、接收数据

<!-- more -->

## 原生 Ajax 的工作流程

1. 创建XMLHttp对象

```js
var xmlhttp = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP")

标准浏览器：使用XMLHttpRequest对象
IE浏览器：用ActiveXObject对象
```

2. 调用对象的 open方法 来规定请求的类型、URL 以及是否异步处理请求


```js
open(method, url, async)

method：请求的类型 GET/POST
url：文件在服务器上的位置
async：true（异步） / false（同步）
```

3. 如果为异步请求，使用 **onreadystatechange事件** 来监听请求的状态，当 **readyState 属性为 4** 时，就会调用 执行函数

```js
readyState
0：请求未初始化
1：服务器连接已建立
2：请求已接收
3：请求处理中
4：请求已完成，且相应已就绪
```

4. 最后调用 send() 将请求发给服务器


```js
// 1、创建XMLHttpRequest对象
var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");

// 2、设置请求参数
xhr.open(请求方式,请求地址,异步或同步);

// 3、设置回调函数
xhr.onreadystatechange = function(){
    if(xhr.reasyState===4){
        if(xhr.status === 200) {
        	//5、接受响应
        	console.log(xhr.responseText);
        }
    }
}

// 4、发送请求
xhr.send();
```

### 自定义header的字段

发送之前用 **setRequestHeader(header, value)** 向请求添加http头



## 跨域携带cookie

跨域请求时，由于浏览器安全 策略，不会自动携带cookie到服务器

### 原生 Ajax 开启

我们可以设置 XMLHttpRequest.withCredentials ， xmlhttp.setRequestHeader('Cookie', document.cookie) 是不安全的做法

```js
XMLHttpRequest.withCredentials=true;
xmlhttp.setRequestHeader('Cookie', document.cookie) // error
```

### Jq -> Ajax开启

```js
 $.ajax({
    type: "POST",
    url: "http://xxx.com/api/test",
    dataType: 'jsonp',
    xhrFields: {withCredentials: true},
    crossDomain: true,
})

服务器端设置：
header("Access-Control-Allow-Credentials: true");
header("Access-Control-Allow-Origin: http://www.xxx.com")
```



## 用途 / 好处

- 实现客户端服务器的异步通信效果，带来更好的用户体验
- 实现页面的局部刷新，按需获取数据，节约带宽资源



## 缺点

- 不支持浏览器back按钮
- 安全问题，暴露了与服务器交互的细节
- 对搜索引擎的支持比较弱：如果使用不当，会增大网络数据的流量，从而降低整个系统的性能