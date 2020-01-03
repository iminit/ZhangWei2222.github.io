---
title:  Git填坑笔记
date: 2019-07-03 15:58:37
categories:
- Git
tags:
- 填坑
comments: false
---

#### 缺少权限
```shell
git push ![remote rejected] 分支名->分支名 (pre-receive hook declined) 
```



#### 2. 强制Push失败，提示分支被保护

```shell
you are not allowed to push code to protected branches on this project
```

需要工程的创建人在 git 上把用户添加到被保护的分支的 Developer 中去。

在工程页面，选择设置，下拉选择 `Protected Branches -> Developer`



#### 3. 装oh-my-zsh时总是报错

```shell
localhost% sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" 
curl: (7) Failed to connect to raw.githubusercontent.com port 443: Operation timed out
```

解决：配置git账号

```shell
git config --global user.name 'xx'
git config --global user.email 'xx@xxx.com'
git config -l
```



#### 4. 直接copy库，import后，报错

> Cannot assign to read only property 'exports' of object '#<Object>'

**原因：** 在webpack打包的时候，可以在js文件中混用require和export。但是不能混用import 以及module.exports。因为webpack 2中不允许混用import和module.exports

**解决：** 把库的`index.js` module.exports 改成 export default function...



#### 5. 在html文件中怎么引入库呢？直接在`script`引入地址会报错

> Uncaught SyntaxError: Cannot use import statement outside a module

**解决：**

```html
<script type="module">
  import a from './库/index.js'
</script>

// 或者 
<script src='./库/index.js' type="module"></script>
```