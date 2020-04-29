---
title: markdown 编写文件目录结构
date: 2020-04-29 11:51:48
categories:
- 开发
comments: false
---

1. 先全局安装 tree

    ```bash
    npm i tree-node-cli -g
    ```

2. 查看帮助文档

    ```bash
    tree --help
    ```

3. 先 cd 到需要生成目录的文件夹下，输入

    ```bash
    tree -L 4 -I "node_modules" > README.md
    # -L 是确定要几级目录，-I是排除哪个文件夹下的，在 README 里面生成项目结构树
    ```