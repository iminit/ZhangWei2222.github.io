---
title: Vue+Koa2+MySQL 全栈开发记录
date: 2020-04-29 17:04:47
categories:
- 全栈
comments: false
---

以毕业项目：青年旅社移动端 的登录注册为例，记录 Vue+Koa2+MySQL 全栈开发过程

[Github地址](https://github.com/ZhangWei2222/Youth-Hostel)

<!-- more -->



### 相关目录介绍

```
Youth-Hostel
├── client        // 前端 Vue 工程
│   ├── src
│   │   ├── router      // 路由
│   │   │   └── index.ts
│   │   ├── services    // 定义API
│   │   │   ├── lib
│   │   │   │   └── http.ts   // 对 axios 设置 request 拦截器和 response 拦截器
│   │   │   └── loginAPI.ts
├── db                  // 后端 Node 工程
│   ├── config
│   │   └── default.ts  // 数据库配置文件
│   ├── index.ts        // 入口文件
│   ├── lib             // 封装各 sql 语句， 连接 MySQL
│   │   ├── dbtools.ts       // sql组装模块
│   │   └── mysql.ts         // 房客相关（包括登录、注册）
│   ├── routers         // 路由，调用并执行在 lib 中对应的方法
│   │   ├── signIn.ts        // 登录相关
│   │   └── signUp.ts        // 注册相关
│   └── token
│       ├── checkToken.ts    // 检查token
│       └── createToken.ts   // 创建token
├── logs                // 分类别、按时间存放后端日志
│   ├── debug
│   ├── error
│   ├── info
│   │   ├── info_file.2020-03-02.log
│   │   ├── ...
│   │   └── info_file.2020-04-23.log
│   ├── log_config.json      // log4js 配置文件
│   └── trace
├── package-lock.json
├── package.json
├── public
└── README.md
```



### [log4js 配置](http://super-wei.top/2020/04/26/%E6%9C%8D%E5%8A%A1%E5%99%A8/log4js%20%E9%85%8D%E7%BD%AE/)

首先要注入日志管理工具，这里选择了 log4js ，可以分类别、按日期地记录各日志，方便调试、debugger



### 数据库配置

`db/config/default.ts` 中进行数据库配置

```typescript
module.exports = {  // mysql 配置
    database: {
        host: '101.133.xxx.xxx', // 服务器ip地址
        user: 'root', // 用户账号
        password: 'xxxxxx', // 密码
        database: 'youth_hostel', // 数据库名
        multipleStatements: true // 允许每个mysql语句有多条查询.使用它时要非常注意，因为它很容易引起sql注入攻击(默认:false)
    },
    port: 4442 // 监听端口
}
```



### Koa2 配置

`db/index.ts` 中编写入口文件，完成服务器连接和日志服务

```typescript
const globalAny: any = global;
const path = require('path')
const Koa = require('koa2');
const app = new Koa();
const router = require('koa-router');
var cors = require('koa2-cors');
const bodyParser = require('koa-bodyparser');
const config = require('./config/default.ts');
const server = require('koa-static');
require('./lib/mysql.ts');

// // 设置静态文件
// app.use(server(
//   path.join(__dirname, '../dist')
// ))

// 设置跨域
app.use(cors({
    origin: function (ctx) { // 设置允许来自指定域名请求
        if (ctx.request.header.referer === "http://localhost:8080/") {
            return 'http://localhost:8080';
        } else {
            return 'http://101.133.xxx.xxx';
        }
    },
    maxAge: 5, //指定本次预检请求的有效期，单位为秒。
    credentials: true, //是否允许发送Cookie
    allowMethods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'], //设置所允许的HTTP请求方法
    allowHeaders: ['Content-Type', 'Authorization', 'Accept'], //设置服务器支持的所有头信息字段
    exposeHeaders: ['WWW-Authenticate', 'Server-Authorization'] //设置获取其他自定义字段
})
       );

// ctx body 中间件
app.use(bodyParser({
    formLimit: '1mb'
}))

// 各路由
app.use(require('./routers/signUp.ts').routes()) // 注册
app.use(require('./routers/signIn.ts').routes()) // 登录

// 监听端口
app.listen(config.port, () => {
    console.log("——————————服务已启动——————————");
})

// 开启日志服务
const log4js = require("log4js");
const log4js_config = require("../logs/log_config.json");
log4js.configure(log4js_config);
globalAny.log = log4js.getLogger("wei");

globalAny.log.info('---开始listen in localhost:' + config.port);
export { };
```



### 编写 SQL 拼接模板

`db/lib/dbtools.ts`编写 [SQL 拼接模板](http://super-wei.top/2020/04/26/%E6%95%B0%E6%8D%AE%E5%BA%93/%E7%AE%80%E5%8D%95SQL%E8%AF%AD%E5%8F%A5%E6%8B%BC%E6%8E%A5%E6%A8%A1%E6%9D%BF-Node.js%E7%89%88/)，之后在每个子文件中调用模板进行拼接，可以大大提高 sql 语句的编写效率，如`db/lib/mysql.ts`

```typescript
const globalAny: any = global;
const mysql = require('mysql');
const config = require('../config/default.ts');
const { getSQLObject, _structureAnalysis } = require('../lib/dbtools.ts');
var pool = mysql.createPool(config.database);

const query = function (sql, val) {
    return new Promise((resolve, reject) => {
        pool.getConnection(function (err, connection) {
            if (err) {
                reject(err)
            } else {
                connection.query(sql, val, (err, res) => {
                    if (err) {
                        reject(err)
                    } else {
                        resolve(res)
                    }
                    connection.release();
                })
            }
        })
    })
}

const findUser = (val) => { // 查找所有User
    let stru = getSQLObject();
    stru["query"] = "select";
    stru["tables"] = "users";
    stru["data"] = {
        "*": '*'
    };
    stru["where"]["condition"] = [
        "name = '" + val + "'",
    ];
    let result = _structureAnalysis(stru);
    globalAny.log.trace("[findUser] sql语句: " + result.sql + " value参数: " + result.value);
    return query(result.sql, result.value)
}

module.exports = {
    //暴露方法
    findUser
}
export { };
```



### 分析登录 注册逻辑

1. 注册 -> 判断是否用户名存在->（否）sql 注册
2. 登录 -> 判断账号是否存在 -> （是）校验密码 -> 成功 -> 返回Toke



### 后端编写

#### 注册

##### 编写路由

`db/router/signUp.ts`

```typescript
const globalAny: any = global;
const router = require('koa-router')();
const userModel = require('../lib/mysql.ts');

// 注册
router.post('/api/signUp', async (ctx, next) => {
    // post请求 从body中获取注册参数
    let user = {
        name: ctx.request.body.name,
        password: ctx.request.body.password,
        sex: ctx.request.body.sex
    }

    await userModel.findUser(user.name).then(async (res) => {
        if (res.length) { // length > 1 说明 表中有数据
            globalAny.log.error("[insetUser] 注册失败: 用户已存在！");
            ctx.body = {
                code: 102,
                msg: '用户已存在!',
                data: []
            }
        } else {  // 否者没有注册
            await userModel.insetUser(user).then((res) => {
                globalAny.log.trace("[insetUser] 注册成功");
                ctx.body = {
                    code: 0,
                    msg: '注册成功!',
                    data: res
                }
            }).catch((err) => {
                globalAny.log.error("[insetUser] 注册失败: " + err);
                ctx.body = {
                    code: -1,
                    msg: err
                }
            })
        }
    })
})
module.exports = router
export { };
```

##### 编写 SQL 语句

`db/lib/mysql.ts`

```typescript
const insetUser = (val) => { // 注册
    let stru = getSQLObject();
    stru["query"] = "insert";
    stru["tables"] = "users";
    stru["data"] = {
        "name": val.name,
        "password": val.password,
        "sex": val.sex
    };
    let result = _structureAnalysis(stru);
    globalAny.log.trace("[insetUser] sql语句: " + result.sql + " value参数: " + result.value);
    return query(result.sql, result.value)
}
```

##### 解析 token 

`db/token/checkToken.ts`

```typescript
const jwt = require('jsonwebtoken');

// 接口访问必须要有Token (需要用户登录)
module.exports = async (ctx, next) => {
    const authorization = ctx.get('Authorization'); // request 带过来的 token  存在浏览器的cookie中
    if (authorization == '') {
        ctx.body = {
            code: 104,
            msg: '未登录'
        }
        return false
    }
    const token = authorization;
    let tokenContent;
    try {
        // 根据 "wei" 钥解析 token 判断是否失效
        tokenContent = await jwt.verify(token, 'wei');     //如果token过期或验证失败，将抛出错误
        // 存入ctx 中 next() 可以获取到设置的 userInfo 数据
        ctx.userInfo = tokenContent
    } catch (err) {
        ctx.body = {
            code: 0,
            msg: '登录验证失效'
        }
    }
    await next();
}
export { }
```

#### 登录
##### 编写路由

`db/router/signIn.ts`

```typescript
const globalAny: any = global;
const router = require('koa-router')();
const userModel = require('../lib/mysql.ts');
const createToken = require('../token/createToken.ts');

router.post('/api/signIn', async (ctx, next) => {
  let user = {
    username: ctx.request.body.username,
    password: ctx.request.body.password
  }

  await userModel.findUser(user.username).then((res) => {
    if (!res.length) {
      ctx.body = {
        code: 101,
        msg: '用户未注册!',
        data: []
      }
      globalAny.log.error("[findUser] 房客未注册!");
    } else {
      if (res[0].password === user.password) {
        let token = createToken(res[0]) // 创建token 存储用户id等重要信息

        ctx.body = {
          code: 0,
          msg: '用户登录成功!',
          data: [],
          token
        }
        globalAny.log.trace("[findUser] 登录成功!");
      } else {
        ctx.body = {
          code: 103,
          msg: '用户名或者密码错误!',
          data: []
        }
        globalAny.log.error("[findUser] 用户名或者密码错误!");
      }
    }
  }).catch((err) => {
    ctx.body = {
      code: -1,
      msg: err,
      data: []
    }
    globalAny.log.error("[findUser] 登录失败:" + err);
  })
})
```

##### 编写 SQL 语句

`db/lib/mysql.ts`

```typescript
const findUser = (val) => { // 查找所有User
  let stru = getSQLObject();
  stru["query"] = "select";
  stru["tables"] = "users";
  stru["data"] = {
    "*": '*'
  };
  stru["where"]["condition"] = [
    "name = '" + val + "'",
  ];
  let result = _structureAnalysis(stru);
  globalAny.log.trace("[findUser] sql语句: " + result.sql + " value参数: " + result.value);
  return query(result.sql, result.value)
}
```

##### 生成 token

`db/token/createToken.ts`

```typescript
const jwt = require('jsonwebtoken');

//登录时：核对用户名和密码成功后，应用将用户的id 作为JWT Payload的一个属性
module.exports = function (user) {

    // jwt.sign 参数详情
    //第一个是Payload，也就是用户信息（要注意payload不要传整个文档，Payload需要的是唯一且不变的数据，否则当Payload改变的时候需要重新下发token）。这里我们用文档的id，目的是唯一标识用户

    // 第二个参数是密钥，也就是你生成Signature时所用到的加密密钥。要注意这里必须和创建jwt的时候传入的secret一致，因为服务端需要用创建时的secret来解密。

    // 第三个参数则是设置一个token的过期时间，这里我们设置的是1天。

    const token = jwt.sign({
        userId: user.id
    }, 'wei', {    // "wei"  是校验码    解析时需要一致 才能取到 user 信息
        expiresIn: '24h' //过期时间设置为24h 格式有(s, m, h , day)。那么decode这个token的时候得到的过期时间为 : 创建token的时间 +　设置的值
    });
    return token; // 返回token  前端存在浏览器cookie 中
};

export { }
```



### 前端编写

#### 编写拦截器

`client/src/services/lib/http.ts`，对 axios 设置 request 拦截器和 response 拦截器

```typescript
import axios from 'axios'
import NProgress from 'nprogress'
import cookie from 'js-cookie'
import 'nprogress/nprogress.css'

var instance = axios.create();
NProgress.configure({ showSpinner: false });

instance.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
// 设置接口转发地址
const urlDev = 'http://localhost:4442'
const urlQa = 'http://101.133.xxx.xxx:4442'
process.env.NODE_ENV === 'development' ?
    instance.defaults.baseURL = urlDev :
instance.defaults.baseURL = urlQa

// http request 拦截器
instance.interceptors.request.use(
    config => {
        config.withCredentials = true
        NProgress.start()
        config.headers['Authorization'] = cookie.get('assent_token') || ''; // 设置assent_token
        return config;
    },
    err => {
        return Promise.reject(err);
    });

// http response 拦截器
instance.interceptors.response.use(
    response => {
        NProgress.done()
        if (response.data.msg === "登录验证失效") {
            cookie.remove('assent_token')
        }
        return response;
    },
    error => {
        NProgress.done()
        return Promise.reject(error)
    });

export default instance
```

#### 编写 API 接口

`client/src/services/loginAPI.ts`

```typescript
import http from './lib/http'

export const signUpAPI = params => {
    return http.post('/api/signUp', params);
}

export const signInAPI = params => {
    return http.post('/api/signIn', params);
}
```

#### 修改前端路由

可以通过配置前端路由进行**登录权限的约束**：每次路由跳转前判断是否需要登录权限，如果需要登录权限，再判断浏览器是否存在 token，如果没有则跳转到登录页面，如`client/src/router/index.ts`

```typescript
import Vue from "vue";
import Router from "vue-router";
const routerPush = Router.prototype.push
Router.prototype.push = function push(location) {
  return routerPush.call(this, location).catch(error => error)
}
import cookie from 'js-cookie'
const views: any = require['context']('../views', true, /\.vue$/im);
const modules: any = {}

function getFloorConfigName(path): any {
  return path.match(/([\w-]+)\.vue/)[1];
} //([\w-]+) ： 匹配数字和字母下划线，中划线的多个字符
views.keys().forEach(key => {
  modules[getFloorConfigName(key)] = views(key).default || views(key)
})

Vue.use(Router);

const router = new Router({
  mode: 'hash',
  routes: [
    {
      path: "/",
      redirect: '/home',
    },
    {
      path: '/signIn',
      name: 'SignIn',
      component: modules['SignIn']
    },
    {
      path: '/signup',
      name: 'SignUp',
      component: modules['SignUp']
    }
  ],
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    }
    return { x: 0, y: 0 }
  }
});

router.beforeEach((to, from, next) => {
  let token = cookie.get('assent_token');
  if (to.meta.requireAuth) {
    if (token) {
      next();
    } else {
      next({
        path: '/signIn',
        query: { redirect: to.fullPath }  // 将跳转的路由path作为参数，登录成功后跳转到该路由
      });
    }
  } else {
    next();//如果无需token,那么随它去吧
  }
});
export default router
```

#### 注册

```typescript
async signUp(): Promise<any> {
    let self = this;
    let userForm = this.$refs["UserForm"].userForm;
    let params = {
        name: userForm.name,
        password: userForm.password && md5(userForm.password),
    };
    const res = await signUpAPI(params);
    try {
        console.log("注册信息" + JSON.stringify(res.data));
        if (res.data.code === 0) {
            Toast.success(res.data.msg);
            self.$router.replace("/signIn"); // 跳转到登录页面
        } else {
            Toast.fail("注册失败" + res.data.msg);
        }
    } catch (error) {
        Toast.fail("注册失败");
        console.log("注册失败" + error);
    }
}
```

#### 登录

```typescript
async signIn(): Promise<any> {
    let self = this;
    if (self.username && self.password) {
        let res;
        res = await signInAPI({
            username: self.username,
            password: md5(self.password)
        });
        try {
            console.log("用户登录信息" + JSON.stringify(res.data));
            if (res.data.code === 0) {
                Toast.success(res.data.msg);
                cookie.set("assent_token", res.data.token, {  // 设置 token
                    expires: 1,
                    path: ""
                });
                if (self.$route.query.redirect) {  // 跳转判断
                    self.$router.replace("" + self.$route.query.redirect);
                } else {
                    self.$router.replace("/");
                }
            } else {
                Toast.fail(res.data.msg);
            }
        } catch (error) {
            Toast.fail("用户登录失败");
            console.log("用户登录失败" + error);
        }
    }
}
```



### 参考文章

[全栈前端入门必看 koa2+mysql+vue+vant 构建简单版移动端博客](https://segmentfault.com/a/1190000015962701)



