---

title: Http
date: 2019-03-11 10:05:48
categories:
- 计算机网络
tags:
- Http
comments: false
---

## 状态码

<!-- more -->

- 206：在客户端表明自己只需要目标url上的部分资源
- 301：永久性重定向，表示请求的资源已经永久的搬到了其他位置
- 302：临时重定向，表示请求的资源临时搬到了其他位置
- 304：缓存
- 400：请求无效（出现这个请求无效报错说明请求没有进入到后台服务里） 
  - 前端提交数据的字段名称或者是字段类型和后台的实体类不一致，导致无法封装
  -  前端提交的到后台的数据应该是json字符串类型，而前端没有将对象转化为字符串类型
- 403：请求的对应资源禁止被访问
- 404：服务器无法找到对应资源
- 500：服务器内部错误

- 503：服务器正忙
- 504：网关超时

### 301 与 302

**相同**

表示重定向，浏览器在拿到服务器返回的这个状态码后会自动跳转到一个新的URL地址，

**区别**

- 301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址；

- 302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，搜索引擎会抓取新的内容而保存旧的网址

**跳转场景**
- 301
    - 更换域名
    - 在搜索引擎的搜索结果中出现了不带www的域名，而带www的域名却没有收录，这个时候可以用301重定向来告诉搜索引擎我们目标的域名是哪一个。
    - 空间服务器不稳定，换空间的时候。
- 302
	-  尽量使用301跳转！不然会有网址规范化和网址劫持



## Http 定义

- 超文本传输协议，默认端口号为80；
- 构建与TCP/IP协议上，处于应用层
- 无连接无状态的
- 采用的是请求/响应的工作方式
- 通过报文的方式来交换数据，分为请求报文和响应报文
  - 请求报文由请求行（包含请求方法、URI、HTTP版本信息），请求头，请求体组成；
  - 响应报文包括状态行（包含HTTP版本、状态码、状态码的原因短语），响应头和响应体组成

### 什么叫无连接无状态
- 无连接是指限制每次连接只处理一个请求：服务器处理完客户的请求，并收到客户的应答后，即断开连接。这种方式可以节省传输时间
- 无状态是指协议对于事务处理没有记忆能力，服务器处理完请求后，不会记录任何信息，每个请求都是独立的



## Http 头

```js
Host:rss.sina.com.cn //请求资源所在的服务器
User-Agent：Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.8.1.14) Gecko/20080404 Firefox/2.0.0.14 //浏览器自己的信息
Accept：text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5 //可处理的媒体类型
Accept-Language：zh-cn,zh;q=0.5  //可接收的的语言
Accept-Encoding：gzip,deflate  //可接收的的编码方式
Accept-Charset：gb2312,utf-8;q=0.7,*;q=0.7 //可接收的字符集
Keep-Alive：300 //保持多久
Connection：keep-alive //连接的状态
Cookie：userId=C5bYpXrimdmsiQmsBPnE1Vn8ZQmdWSm3WRlEB3vRwTnRtW <-- Cookie
If-Modified-Since：Sun, 01 Jun 2008 12:05:30 GMT //缓存
Cache-Control：max-age=0
```



## 8种请求方式

|         | 方式    | 作用                                                         |
| ------- | ------- | ------------------------------------------------------------ |
| Http1.0 | GET     | **获取**资源，请求参数在**请求行**中                         |
|         | HEAD    | 类似 **GET 请求**，但返回的响应没有具体内容，用于**获取报头** |
|         | POST    | **提交**资源，请求参数在**请求体**中                         |
| Http1.1 | PUT     | **更新**资源                                                 |
|         | DELETE  | **删除**指定资源                                             |
|         | OPTIONS | **查询**服务器性能、服务器支持的 Http 请求方法               |
|         | TRACE   | 请求服务器**回送**的请求信息，主要用于**测试或诊断**（一般禁用，防止被恶意攻击或盗取信息） |
|         | CONNECT | 能够将连接改为**管道方式**的代理服务器（把服务器作为跳板，去访问其他页面，然后再把数据返回回来，连接成功后，就可以正常的 GET 或 POST...） |



### Get 和 Post请求方式 的区别
- 安全性不同：Get请求的安全性较差，因为数据参数是直接拼接在URL后面的，可见。Post请求的数据参数是封装在 Http 的请求体中，浏览器中没有缓存。

- 参数长度不同：Get请求可提交的数据量收到URL长度的限制，不同浏览器对长度的限制不同，IE为2048个字符（最小）。Post请求是没有长度的限制的。

- Get请求适用于数据量较小，并且数据不敏感的情况。Post正好相反

- Get请求的数据类型只允许Ascll字符，Post可以是任何类型



### POST、PUT、PACTH 区别

#### 幂等
相同的操作重复执行多次，效果都是一样的

| POST                                             | PUT                                    | PATCH                                      |
| ------------------------------------------------ | -------------------------------------- | ------------------------------------------ |
| 用来在服务器上创建一个子资源                     | 更新                                   | 对 PUT 方法的补充，局部更新                |
| **不幂等**：多次执行，将导致多条相同的条目被创建 | **幂等**：存在则替换，不存在则创建     | **幂等**                                   |
|                                                  | 需要把**完整的对象**传给后端，进行更新 | 只需要把**要更新的字段**传给后端，完成更新 |



## 文件上传到后端发生了什么
1. 前端通过浏览器获取到文件地址，然后用Post协议，将文件切片逐步上传，直到传输完成
2. 后端通过 post协议，逐步获取到文件数据片，在服务器磁盘缓存区中将片逐步组装，完成后将文件保存到指定位置并返回前端传输成功，关闭连接



## Http1.1 与 Http1.0 区别

- **缓存处理，引入更多的缓存头**：1.0主要使用 Expires、If-Modified-Since来作为缓存判断的标准，1.1有了 E-tag、If-Match、If-None-Match等
- **带宽优化**：1.0中，虽然我们只是需要某个对象的一部分，服务器也会把整个对象传过来，并且不支持 断点续传 功能；1.1在请求头引入了 range头域，允许只请求资源的某个部分，返回码是206
- **新增24个错误状态码**：如 410，表示服务器上的某个资源永久地被删除
- **Host头处理**：1.1请求和响应都需要支持 Host头域，传递主机名，因为现在一个 ip 可能会有很多个主机，如果没有 Host头域 ，会报 400
- **长连接**：1.1请求头中默认开启 Connection: Keep-Alive，在一个 TCP 连接上可以传送多个 HTTP请求和响应



## Http2 与 Http1 区别

- 采用**二进制格式**而非文本格式
- **多路复用**：连接共享。1.1 虽然采用了长连接，一个 TCP连接上可以传输多个 HTTP 请求和响应，但是**多个请求串行化单线程处理**，后面的请求需要等待前面的请求返回才能执行，一旦某请求超时，后续请求会被阻塞；2 就可以让多个请求在一个连接上并行执行。即使某请求超时，也不影响其他连接的正常执行
- **使用报头压缩，降低开销**：通讯双方各自 cache 一份 header fields 表，避免重复传输 header，减少需要传输的大小
- **服务器推送**：能把客户端需要的资源伴随 index.html 一起发送过来，省去了客户端重复请求的步骤，极大地提升了速度



## 关于 TCP

- tcp可以分段传输信息，有个机制，不会丢包或者乱序
- http1.1的时候，一个域名下只能有6个tcp连接，不过一个tcp连接可以有多个http请求