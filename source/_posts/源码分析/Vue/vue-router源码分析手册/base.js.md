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

负责控制所有的路由守卫的执行

```js
confirmTransition(route: Route, onComplete: Function, onAbort?: Function) {
    const current = this.current;

    // 中断跳转路由函数
    const abort = err => {
      // after merging https://github.com/vuejs/vue-router/pull/2771 we
      // When the user navigates through history through back/forward buttons
      // we do not want to throw the error. We only throw it if directly calling
      // push/replace. That's why it's not included in isError
      if (!isExtendedError(NavigationDuplicated, err) && isError(err)) {
        if (this.errorCbs.length) {
          this.errorCbs.forEach(cb => {
            cb(err);
          });
        } else {
          warn(false, "uncaught error during route navigation:");
          console.error(err);
        }
      }
      onAbort && onAbort(err);
    };

    // 如果是相同 直接返回
    if (
      isSameRoute(route, current) &&
      // in the case the route map has been dynamically appended to
      route.matched.length === current.matched.length
    ) {
      this.ensureURL();
      return abort(new NavigationDuplicated(route));
    }

    // 下面分析
    const { updated, deactivated, activated } = resolveQueue(
      this.current.matched,
      route.matched
    );

    // 下面分析
    const queue: Array<?NavigationGuard> = [].concat(...);

    // 下面分析
    const iterator = (hook: NavigationGuard, next) => {
      // ...
    };

    // 下面分析
    runQueue(queue, iterator, () => {
      // ...
    });
  }
```



#### 路由守卫的原理

和组件的生命周期的钩子不同，路由守卫将重点放在路由上，**能够控制路由跳转，一般用在页面级别的路由跳转时控制跳转的逻辑**，比如在路由守卫中检查用户是否有进入当前页面的权限，没有则跳转到授权页面，亦或是在离开页面时警告用户有未确认的信息，确认后才能跳转等等

在路由守卫中，一般会接收3个参数，to，from，next，前两个分别是跳转后和跳转前页面路由的 $route 对象，第三个参数 next 是一个函数，当执行 next 函数后会进行跳转，如果一个包含 next 参数的路由守卫里没有执行该函数，**页面会无法跳转**。

#### ` resolveQueue`函数 获取所有需要激活、更新、销毁的路由

```js
const { updated, deactivated, activated } = resolveQueue(
  this.current.matched,
  route.matched
);
```

根据跳转前和跳转后的route对象的`matched数组`(当前 $route 对象以及所有父级的路由记录)，返回这2个数组包含的路由记录的区别

```js
function resolveQueue(
  current: Array<RouteRecord>,
  next: Array<RouteRecord>
): {
  updated: Array<RouteRecord>,
  activated: Array<RouteRecord>,
  deactivated: Array<RouteRecord>
} {
  let i;
  const max = Math.max(current.length, next.length);
  for (i = 0; i < max; i++) {
    // 当前路由路径和跳转路由路径不同时跳出遍历
    if (current[i] !== next[i]) {
      break;
    }
  }
  return {
    // 可复用的组件对应路由
    updated: next.slice(0, i),
    // 需要渲染的组件对应路由
    activated: next.slice(i),
    // 失活的组件对应路由
    deactivated: current.slice(i)
  };
}
```



#### `queue`函数 获取所有需要执行的路由守卫

数组中的守卫排列顺序是设计好的，对应vue-router官方文档中提到的路由导航解析流程

1. 导航被触发
2. 在失活的组件里调用离开守卫
3. 调用全局的`beforeEach`守卫
4. 在重用的组件里调用`beforeRouteUpdate`守卫(2.2+)
5. 在路由配置里调用`beforeEnter`
6. 解析异步路由组件
7. 在被激活的组件里调用`beforeRouteEnter`
8. 调用全局的`beforeResolve`守卫(2.5)
9. 导航被确认
10. 调用全局的`afterEach`钩子
11. 触发DOM更新
12. 用创建好的实例调用`beforeRouteEnter`守卫中传给`next`的回调函数

```js
/* NavigationGuard是一个标准的路由守卫的签名，经过 queue 数组内部这些函数的转换最终会返回路由守卫组成的数组
    declare type NavigationGuard = (
      to: Route,
      from: Route,
      next: (to?: RawLocation | false | Function | void) => void
    ) => any
    */
const queue: Array<?NavigationGuard> = [].concat(
  // 失活的组件钩子
  extractLeaveGuards(deactivated),
  // 全局 beforeEach 钩子
  this.router.beforeHooks,
  // 在当前路由改变，但是该组件被复用时调用
  extractUpdateHooks(updated),
  // 需要渲染组件 enter 守卫钩子
  activated.map(m => m.beforeEnter),
  // 解析异步路由组件
  resolveAsyncComponents(activated)
);
```

