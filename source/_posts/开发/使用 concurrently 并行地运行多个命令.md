---
title: 使用 concurrently 并行地运行多个命令
date: 2020-04-26 17:56:48
categories:
- 开发
comments: false
---

项目目录结构如下，想要实现一个命令同时跑前端和后端，可以使用 **concurrently**

<!-- more -->

```markdown
.
├── client        // 前端代码
├── db            // 后端代码
├── logs          // 日志
├── node_modules
├── package-lock.json
├── package.json
└── README.md
```



1. 在项目根目录安装

   ```bash
   npm i concurrently --save
   ```

2. 修改 `package.json`

   ```json
   "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "client": "npm start --prefix client",
       "server": "nodemon ./db/index.ts",
       "dev": "concurrently \"npm run server\" \"npm run client\""
   }
   ```

3. 执行

   ```bash
   npm run client # 执行前端代码
   npm run server # 执行后端代码
   npm run dev    # 同时执行前面两条代码
   ```

   

