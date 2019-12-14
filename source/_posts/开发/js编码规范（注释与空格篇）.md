---
title: js编码规范（注释与空格篇）
date: 2019-07-08 17:49:37
categories:
- 开发
tags:
- 编码规范
comments: false
---

## 注释
### 多行注释使用 /* … /

```javascript
/**
* @param {Grid} grid 需要合并的Grid
* @param {Array} cols 需要合并列的Index(序号)数组；从0开始计数，序号也包含。
* @param {Boolean} isAllSome 是否2个tr的cols必须完成一样才能进行合并。true：完成一样；false(默认)：不完全一样
* @return void
* @author 单志永 2018/11/8
*/
function mergeCells(grid, cols, isAllSome) {
    // Do Something
}
```

### 单行注释使用 //
注释为一行，且内容前面需要有空格

```javascript
// is current tab
const active = true;
```

## 空格

### 使用 2 个空格作为缩进，在大括号前放置 1 个空格

```javascript
function test() {
  console.log('test');
}
```

### 在控制语句（if、else、while 等）的小括号前放一个空格


```javascript
// good
if (isJedi) {
  fight();
}
```

### 使用空格把运算符隔开


```javascript
// good
const x = y + 5;
```

### 不要在圆括号内加空格

```javascript
// good
function bar(foo) {
  return foo;
}
```

### 不要在中括号内添加空格

```javascript
// good
const foo = [1, 2, 3];
console.log(foo[0]);
```

### 在大括号内添加空格

```javascript
// good
const foo = { clark: 'kent' };
```