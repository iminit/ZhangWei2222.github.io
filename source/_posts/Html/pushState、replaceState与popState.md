---
title:  HTML5的pushState、replaceState与popState
date: 2019-12-25 14:11:37
categories:
- Html
comments: false
---



## 简介

HTML5引入了 [history.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState) 和 [history.replaceState()](https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_replaceState()_method) 方法，它们分别可以添加和修改历史记录条目。这些方法通常与[`window.onpopstate`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/onpopstate) 配合使用。

<!-- more -->

## `pushState()`方法的例子

1. 假设在 http://mozilla.org/foo.html 中执行了以下 JavaScript 代码，浏览器地址显示为 http://mozilla.org/bar.html，但并不会导致浏览器加载 `bar.html` ，甚至不会检查`bar.html` 是否存在

   ```js
   let stateObj = {
       foo: "bar",
   };
   history.pushState(stateObj, "page 2", "bar.html");
   ```
   
2. 假设现在用户访问 http://google.com，然后点击了返回按钮。此时，地址栏将显示 http://mozilla.org/bar.html，`history.state` 中包含了 `stateObj` 的一份拷贝。页面此时展现为`bar.html`。且因为页面被重新加载了，所以`popstate`事件将不会被触发。

3. 如果我们再次点击返回按钮，页面URL会变为http://mozilla.org/foo.html，文档对象document会触发另外一个 `popstate` 事件，这一次的事件对象state object为null。 这里也一样，返回并不改变文档的内容，尽管文档在接收 `popstate` 事件时可能会改变自己的内容，其内容仍与之前的展现一致。



## `pushState()`方法

`pushState()` 需要三个参数: 一个状态对象, 一个标题 (目前被忽略), 和 (可选的) 一个URL. 让我们来解释下这三个参数详细内容：

> **状态对象** — 状态对象state是一个JavaScript对象，通过pushState () 创建新的历史记录条目。无论什么时候用户导航到新的状态，popstate事件就会被触发，且该事件的state属性包含该历史记录条目状态对象的副本。

> **标题** — Firefox 目前忽略这个参数，但未来可能会用到。在此处传一个空字符串应该可以安全的防范未来这个方法的更改。或者，你可以为跳转的state传递一个短标题。

> **URL** — 该参数定义了新的历史URL记录。注意，调用 `pushState()` 后浏览器并不会立即加载这个URL，但可能会在稍后某些情况下加载这个URL，比如在用户重新打开浏览器时。新URL不必须为绝对路径。如果新URL是相对路径，那么它将被作为相对于当前URL处理。新URL必须与当前URL同源，否则 `pushState()` 会抛出一个异常。该参数是可选的，缺省为当前URL。



## `pushState()`方法相比`hash`的优点 

在某种意义上，调用 `pushState()` 与 设置 `window.location = "#foo"` 类似，二者都会在当前页面创建并激活新的历史记录。但 `pushState()` 具有如下几条优点：

-  `pushState()` 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 hash 只可修改 # 后面的部分，故只可设置与当前同文档的 URL
-  `pushState()` 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 hash 设置的新值必须与原来不一样才会触发记录添加到栈钟
-  `pushState()` 通过 stateObject 可以添加任意类型的数据到记录中；而 hash 只可添加短字符串
-  `pushState()` 可额外设置 title 属性供后续使用

注意 `pushState()` 绝对不会触发 `hashchange` 事件，即使新的URL与旧的URL仅哈希不同也是如此。



## `replaceState()`方法

`history.replaceState()` 的使用与 `history.pushState()` 非常相似，区别在于 `replaceState()` 是修改了当前的历史记录项而不是新建一个。



## `popstate`事件

每当处于激活状态的历史记录条目发生变化时，`popstate` 事件就会在对应window对象上触发。 如果当前处于激活状态的历史记录条目是由`history.pushState()`方法创建,或者由`history.replaceState()`方法修改过的, 则`popstate`事件对象的`state`属性包含了这个历史记录条目的state对象的一个拷贝。

我们也可以直接在history对象上获取`state`，如下：

```js
var currentState = history.state;
```

**需要注意的是，调用 `history.pushState()` 或者 `history.replaceState()` 不会触发 `popstate` 事件。 `popstate`事件只会在浏览器某些行为下触发， 比如点击后退、前进按钮(或者在JavaScript中调用`history.back()`、`history.forward()`、`history.go()`方法)。**



## `popstate` 事件的例子

假如当前网页地址为 `http://example.com/example.html` ,则运行下述代码后:

```js
window.onpopstate = function(event) {
  alert("location: " + document.location + ", state: " + JSON.stringify(event.state));
};
//绑定事件处理函数. 
history.pushState({page: 1}, "title 1", "?page=1");    //添加并激活一个历史记录条目 http://example.com/example.html?page=1,条目索引为1
history.pushState({page: 2}, "title 2", "?page=2");    //添加并激活一个历史记录条目 http://example.com/example.html?page=2,条目索引为2
history.replaceState({page: 3}, "title 3", "?page=3"); //修改当前激活的历史记录条目 http://ex..?page=2 变为 http://ex..?page=3,条目索引为3
history.back(); // 弹出 "location: http://example.com/example.html?page=1, state: {"page":1}"
history.back(); // 弹出 "location: http://example.com/example.html, state: null
history.go(2);  // 弹出 "location: http://example.com/example.html?page=3, state: {"page":3}
```



## 参考文章

['pushState'、'replaceState'MDN文档](https://developer.mozilla.org/en-US/docs/Web/API/History_API)([中文翻译版](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API))

['onpopstate'MDN文档](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onpopstate)([中文翻译版](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/onpopstate))