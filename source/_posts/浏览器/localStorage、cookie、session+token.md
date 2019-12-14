---
title:  浏览器存储
date: 2019-03-08 15:32:37
categories:
- 浏览器
tags:
- 存储
comments: false
---

## Cookie、sessionStorage、localStorage 区别

|              | Cookie                                                   | SessionStorage                     | LocalStorage      |
| ------------ | -------------------------------------------------------- | ---------------------------------- | ----------------- |
| 存放数据大小 | 4KB                                                      | 一般为 5 MB                        | 一般为 5 MB       |
| 数据有效期   | 可以设置过期时间                                         | 仅在同源窗口有效，关闭窗口就无效了 | 长期有效          |
| 与服务端通信 | 每次都会携带在 HTTP 头中，如果保存过多数据会带来性能问题 | 仅仅在本地存储，不会和服务端通信   | 同 SessionStorage |
| 用途         | 一般由服务器端生成，用于标识用户身份                     | 用于浏览器端缓存数据               | 同 SessionStorage |
|              | 需要程序员手动封装                                       | 有现成的API                        | 有现成的API       |

#### a.html 与 b.html 通信

- cookie
  - 如果在同域下，可以访问
    - **原理：**默认情况下，只有与创建了cookie的页面在同一个目录或子目录下的网页才可以访问cookie
    - **如果同域不同目录下的解决：**把cookie设置到一个更高级别的目录下

    ```js
    document.cookie = "key=value;path=/"
    ```
	- 如果不同域
	  - **解决：**设置domin
	
	    ```js
	    document.cookie = 'key = value;path=/;domain=csdn.cn'
	    ```
	

> www.csdn.cn/blog/a.html 创建了cookie
>
> www.csdn.cn/blog/b.html 就可以访问这个cookie页(同一目录)
>
> www.csdn.cn/blog/xyz/c.html 也可以访问这个cookie页(字目录)
>
> www.csdn.cn/xx/d.html 就不可以访问

- webStorage
  - **不同浏览器无法共享** loacalStorage 和 sessionStorage 的信息
  - **相同浏览器的不同页面**可以共享 **localStorage**（注意页面需要同域）
  - **不同页面或标签页面无法共享** **sessionStorage**，如果页面中包含多个 iframe标签且属于同源页面，那么他们之间是可以共享 seesionStorage的


## Cookie 与 Session 的区别

1. Cookie 与 Session 出现的原因是：**HTTP 是无状态的**，没办法知道请求的是谁
2. Cookie 保存在**客户端**，也就是浏览器端。每次往服务器发请求，都会在请求头里带着cookie，缺点显而易见，**浪费流量、降低效率**
3. Session 将用户信息保存在**服务端**，只需要**往客户端的 Cookie** 中保存一个 **Session id** 值就可以了。这样每次往服务器发请求，Cookie 的 size 很小，在服务端只需要根据 Cookie 传过来的 Session id 进行**匹配session**，从Session 中获取用户信息就可以了



## Token 作用

#### 防止表单重复提交

1. 当客户端请求页面时，服务器会生成一个**随机数 Token**，并且将 Token 放置到 **Session** 当中
2. 然后将 Token **发给客户端**（一般通过构造 hidden 表单）
3. 下次客户端提交请求时，Token 会**随着表单一起提交**到服务器端
4. （如果应用于“anti csrf攻击”，则）服务器端会对**Token值进行验证**，判断是否**和 Session 中的 Token 值相等**
5. 若相等，则可以证明请求有效，不是伪造的

#### 身份验证

1. 服务器端第一次验证通过后，会将 **Session** 中的 **Token 值更新**下

2. 若用户重复提交，**第二次**的验证判断将**失败**，因为用户提交的表单中的 Token 没变，但服务器端 **Session 中 Token 已经改变了**



## Localstorage太长怎么办

1. 设置的时候，在 **key 加标识**
   - 一些表状态的，不要删；一些常用的缓存超出了一定大小，就清除
2. 同样在 **key 加自己业务的标识**，清除其他业务，前提是无交互的缓存
3. **划分域名**：各域名下的存储空间由各业务组统一规范使用
4. **跨页面传值**：考虑单页应用，优先采用url传数据



## LocalStorage(webStorage)使用时需要注意的问题

1. 兼容性
   - ie8.0及以上才有
2. LocalStorage的值为string类型
   - 在使用的时候要进行类型转换
3. 在浏览器隐私模式下不可读取
4. 不同浏览器无法共享 LocalStorage
5. 不能被爬虫获取
6. 存储过多数据会导致页面变卡
   - LocalStorage 的本质是对字符串的读取



## Cookie 参数

| 参数   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| name   | 必需。规定 cookie 的名称。                                   |
| value  | 必需。规定 cookie 的值。                                     |
| expire | 可选。规定 cookie 的有效期。0为永远有效                      |
| path   | 可选。规定 cookie 的服务器路径。                             |
| domain | 可选。规定 cookie 的域名。                                   |
| secure | 可选。规定是否通过安全的 HTTPS 连接来传输 cookie。如果值为1，则cookie只能在https连接上有效，默认值为0。 |

##  

## Cookie 读写

#### 设置

- 以**键值对的形式**保存的，即 key = value 的格式
- 各个cookie之间一般是以**“;”分隔**

```js
document.cookie = "name = "+ username;  

// 设置过期时间
function setCookie(name,value)  {      
    var Days = 30;      
    var exp = new Date();      
    exp.setTime(exp.getTime() + Days*24*60*60*1000);      
    document.cookie = name + "="+ escape (value) + ";
    expires=" + exp.toGMTString();  
}  
```

#### 读取

- document.cookie.split

```js
假设 Cookie为 name=jack;password=123

var username = document.cookie.split(";")[0].split("=")[1];   
```

#### 删除

- 设置过期时间小于当前时间，那么就会删除该 Cookie
- 还可以在谷歌开发工具 application -> cookies 修改

```js
function delCookie(name)  {      
    var exp = new Date();      
    exp.setTime(exp.getTime() - 1);      
    var cval=getCookie(name);      
    if(cval!=null)          
        document.cookie= name + "="+cval+";
    expires="+exp.toGMTString();  
}  
//使用示例  setCookie("name","hayden");  alert(getCookie("name"));  
```