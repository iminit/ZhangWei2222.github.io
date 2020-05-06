---
title: MySQL-Expression #2 of SELECT list is not in GROUP BY clause and contains  nonaggregated column xxxx
date: 2020-05-06 21:02:47
categories:
- MySQL
comments: false
---

**报错原因**：mysql5.7.x版本，默认是开启了 `only_full_group_by` 模式的，但开启这个模式后，原先的 `group by` 语句就报错。

<!-- more -->

**解决方法**：

```mysql
set @@global.sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```

不过以上方法是改变全局的 `sql_mode` ，对于新建的数据库有效，对于已存在的数据库，需要执行：

```mysql
set sql_mode ='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```

但是使用命令可以修改一个会话中的配置项，在其他会话中是不生效的，最好修改 `my.cnf` 配置文件，在 `[mysqld]` 加：

```bash
sql_mode=STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION;
```

**查询 `sql_mode`**：

```mysql
select @@global.sql_mode;
```



参考文章：[MYSQL5.7版本sql_mode=only_full_group_by问题](https://www.cnblogs.com/kenshinobiy/p/9580701.html)