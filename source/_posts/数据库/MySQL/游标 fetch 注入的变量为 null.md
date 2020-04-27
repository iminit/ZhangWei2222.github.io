---
title: MySQL-游标 fetch 注入的变量为 null
date: 2020-04-27 10:57:47
categories:
- MySQL
comments: false
---

可能是**注入的变量名与表的字段名称重复**，因为MySQL 不分大小写，即使字段名称是大写，变量名称是小写，也会被认为是同一个字符串。