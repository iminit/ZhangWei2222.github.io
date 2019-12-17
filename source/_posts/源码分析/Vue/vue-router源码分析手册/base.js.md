---
title: vue-router源码分析(base.js)
date: 2019-12-17 09:32:47
categories:
- 源码分析
tags:
- Vue
- Vue-router
comments: false
---

## base.js

`src/history/base.js`

定义了`History`类，VueRouter中的history，根据mode，可能是HTML5History、HashHistory或Abstract实例，其中HTML5History、HashHistory等都是继承自History类。

`History`类提供了一些路由操作的基本方法：

1. Listen——监听callback

2. onReady ——监听路由是否ready，ready时，将所有cb装进readyCbs列表

3. onError

4. transitionTo ——路由的跳转，会判断跳转to的路径是否在路由表中：是，才进行组件替换，调用confirmTransition



### 1. `constructor`构造函数

```js
constructor(router: Router, base: ?string) {
  // 获取当前router
  this.router = router;
  // 获取路由base
  this.base = normalizeBase(base);
  // 由createRoute生成的基础路由，path:'/'
  this.current = START;
  this.pending = null;
  this.ready = false;
  this.readyCbs = [];
  this.readyErrorCbs = [];
  this.errorCbs = [];
}
```



### 2. `transitionTo`路由跳转函数

路由的跳转，会判断跳转to的路径是否在路由表中：是，才进行组件替换，调用confirmTransition

```js
transitionTo(
  location: RawLocation,
  onComplete?: Function,
  onAbort?: Function
) {
  // 调用 match 得到匹配的 route 对象
  const route = this.router.match(location, this.current);

  // 确认过渡
  this.confirmTransition(
    route,
    () => {
      // 更新路由信息，对组件的 _route 属性进行赋值，触发组件渲染
      // 调用 afterHooks 中的钩子函数
      this.updateRoute(route);
      // 添加 hashchange 监听
      onComplete && onComplete(route);
      // 子类实现的更新url地址
      // 对于 hash 模式的话 就是更新 hash 的值
      // 对于 history 模式的话 就是利用 pushstate / replacestate 来更新
      // 更新 URL
      this.ensureURL();

      // 只执行一次 ready 回调
      if (!this.ready) {
        this.ready = true;
        this.readyCbs.forEach(cb => {
          cb(route);
        });
      }
    },
    err => {
      // 错误处理
      if (onAbort) {
        onAbort(err);
      }
      if (err && !this.ready) {
        this.ready = true;
        this.readyErrorCbs.forEach(cb => {
          cb(err);
        });
      }
    }
  );
}
```

#### `match`路由匹配函数

 `src/create-matcher.js` 

先看如何匹配路由获得路由信息：判断两步，命名路由，非命名路由，因为带有`params`只能用命名路由引入，非命名路由不用判断`parmas`，

```js
function match(
raw: RawLocation,
 currentRoute?: Route,
 redirectedFrom?: Location
): Route {
  // 序列化 url
  // 比如对于该 url 来说 /abc?foo=bar&baz=qux#hello
  // 会序列化路径为 /abc
  // 哈希为 #hello
  // 参数为 foo: 'bar', baz: 'qux'
  const location = normalizeLocation(raw, currentRoute, false, router);
  const { name } = location;

  // 如果是命名路由，就判断记录中是否有该命名路由配置
  if (name) {
    const record = nameMap[name];
    if (process.env.NODE_ENV !== "production") {
      warn(record, `Route with name '${name}' does not exist`);
    }
    // 没找到表示没有匹配的路由
    if (!record) return _createRoute(null, location);
    // 获取所有必须的params。如果optional为true说明params不是必须的
    const paramNames = record.regex.keys
    .filter(key => !key.optional)
    .map(key => key.name);

    // 参数处理
    if (typeof location.params !== "object") {
      location.params = {};
    }

    if (currentRoute && typeof currentRoute.params === "object") {
      for (const key in currentRoute.params) {
        if (!(key in location.params) && paramNames.indexOf(key) > -1) {
          location.params[key] = currentRoute.params[key];
        }
      }
    }

    location.path = fillParams(
      record.path,
      location.params,
      `named route "${name}"`
    );
    return _createRoute(record, location, redirectedFrom);
  } else if (location.path) {
    // 非命名路由处理
    location.params = {};
    for (let i = 0; i < pathList.length; i++) {
      // 查找记录
      const path = pathList[i];
      const record = pathMap[path];
      // 如果匹配路由，则创建路由
      // pathMap[path] = 路由记录
      if (matchRoute(record.regex, location.path, location.params)) {
        return _createRoute(record, location, redirectedFrom);
      }
    }
  }
  // 没有匹配的路由
  return _createRoute(null, location);
}
```

####  `_createRoute`

根据上面match函数条件的不同创建不同的路由

```js
// 根据条件创建不同的路由
function _createRoute(
record: ?RouteRecord,
 location: Location,
 redirectedFrom?: Location
): Route {
  // 重定向和别名逻辑
  if (record && record.redirect) {
    return redirect(record, redirectedFrom || location);
  }
  if (record && record.matchAs) {
    return alias(record, location, record.matchAs);
  }
  // 创建路由对象
  return createRoute(record, location, redirectedFrom, router);
}
```

#### `createRoute`创建路由对象

`src/util/route.js`

```js
export function createRoute(
  record: ?RouteRecord,
  location: Location,
  redirectedFrom?: ?Location,
  router?: VueRouter
): Route {
  const stringifyQuery = router && router.options.stringifyQuery;

  // 克隆参数
  let query: any = location.query || {};
  try {
    query = clone(query);
  } catch (e) {}

  // 创建路由对象
  const route: Route = {
    name: location.name || (record && record.name),
    meta: (record && record.meta) || {},
    path: location.path || "/",
    hash: location.hash || "",
    query,
    params: location.params || {},
    fullPath: getFullPath(location, stringifyQuery),
    // 根据记录层级的得到所有匹配的 路由记录
    matched: record ? formatMatch(record) : []
  };
  if (redirectedFrom) {
    route.redirectedFrom = getFullPath(redirectedFrom, stringifyQuery);
  }
  // 让路由对象不可修改
  return Object.freeze(route);
}

// 获得包含当前路由的所有嵌套路径片段的路由记录
// 包含从根路由到当前路由的匹配记录，从上至下
function formatMatch(record: ?RouteRecord): Array<RouteRecord> {
  const res = [];
  while (record) {
    res.unshift(record);
    record = record.parent;
  }
  return res;
}
```



### 3. `confirmTransition`确认过渡函数



