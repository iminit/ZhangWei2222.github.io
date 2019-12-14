---
title: cors跨域
date: 2019-11-18 14:43:37
categories:
- 浏览器
tags:
comments: false
---

实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨源通信，即为了**解决跨域问题**。

### 请求类型

- 简单请求
- 非简单请求

简单请求一般包括下面两种情况，如果不同时满足下面两个条件，就是非简单请求。

- 请求方法 
    - HEAD / GET / POST
- HTTP 头信息（不超过以下几个字段）
    - Accept
    - Accept-Language
    - Content-Language
    - Last-Event-ID
    - Content-Type：只限于三个值 application/x-www-form-urlencoded、multipart/form-data、text/plain

### 简单请求的 CORS 流程

当 AJAX 请求是个简单请求，浏览器会自动在头信息中，增加一个 `Origin` 字段。

> `Origin` 字段用来说明本次请求的来源（包括协议 + 域名 + 端口号），服务端根据这个值来决定是否同意此次请求。

当 `Origin` 指定的源不在许可范围，服务器会返回一个正常的 HTTP 回应，但浏览器会在响应头中发现 `Access-Control-Allow-Origin` 字段，便抛出异常。

当 `Origin` 指定的源在许可范围，服务器返回的响应头中会多出几个头信息字段。

三个相关头信息：
- `Access-Control-Allow-Origin`
  
    - 该字段是必须的。表示许可范围的域名，通常有两种值：请求时 Origin 字段的值或者 *（星号）表示任意域名。
- `Access-Control-Allow-Credentials`
    - 该字段可选。布尔值，表示是否允许在 CORS 请求之中发送 `Cookie` 。若不携带 `Cookie` 则不需要设置该字段。
    - 当设置为 `true` 则  `Cookie` 包含在请求中，一起发送给服务器。还需要在 AJAX 请求中开启 `withCredentials` 属性，否则浏览器也不会发送 `Cookie` 。
        ```
        let xhr = new XMLHttpRequest();
        xhr.withCredentials = true;
        ```
- `Access-Control-Expose-Headers`
    - 该字段可选。可以设置需要获取的字段。因为默认 CORS 请求时，`XMLHttpRequest` 对象的`getResponseHeader()`方法只能拿到以下 6 个基本字段：
`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`。

### 非简单请求的 CORS 流程

在非简单请求发出 CORS 请求时，会在正式通信之前增加一次 **“预检”请求（OPTIONS方法）**，来询问服务器，本次请求的域名是否在许可名单中，以及使用哪些头信息。

当 **“预检”请求** 通过以后，才会正式发起 AJAX 请求，否则报错。

#### 预检请求

```
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
User-Agent: Mozilla/5.0...
...
```

**“预检”请求** 信息中包含两个特殊字段：
1. `Access-Control-Request-Method`

该字段是必须的，用来列出浏览器的 CORS 请求会用到哪些 HTTP 方法，上例是 PUT。
2. `Access-Control-Request-Headers`

指定浏览器 CORS 请求额外发送的头信息字段，上例是 X-Custom-Header。

#### 预检响应


```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Content-Type: text/html; charset=utf-8
Connection: Keep-Alive
...
```

当预检请求**通过**以后，在预检响应头中，会返回 `Access-Control-Allow-` 开头的信息，其中 `Access-Control-Allow-Origin` 表示许可范围，值也可以是 `*`。

当预检请求**拒绝**以后，在预检响应头中，不会返回 `Access-Control-Allow-` 开头的信息，**并在控制台输出错误信息**。、、