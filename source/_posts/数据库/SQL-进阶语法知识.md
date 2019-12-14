---
title: SQL-进阶语法知识
date: 2019-08-21 11:19:37
categories:
- 数据库
tags:
- SQL
comments: false
---

## 前言
此篇为一些子句 / 字段等进阶知识拓展



## TOP 子句(规定返回数据的数目)

```sql
SELECT TOP number|percent 列名称 
FROM 表名称
```



## Alias 别名

```sql
表的别名
SELECT 别名.列名
FROM 表名 AS 别名

列的别名
SELECT 列名 AS 别名
FROM 表名
```



## JOIN 连接

- 连接两个或多个表中列之间的关系

### 区别

|      | INNER JOIN ===JOIN                                           | LEFT JOIN                                                    | RIGHT JOIN                                                   | FULL JOIN                                                    |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 描述 | 内连接；返回表中所有匹配条件的行                             | 返回左表的全部行和右表匹配条件的行，如果左表的行并不匹配条件，则这一行的右表对应数据用NULL代替 | 返回右表的全部行和左表匹配条件的行，如果右表的行并不匹配条件，则这一行的左表对应数据用NULL代替 | 返回左表和右表所有的行，如果其中一个表的数据行在另一个表中没有匹配的行，那么对应的数据用NULL代替 |
| 语法 | SELECT * FROM Table A<br/>INNER JOIN Table B<br/>ON Table A.id = Table B.id | SELECT * FROM Table A<br/>LEFT JOIN Table B<br/>ON Table A.id = Table B.id | SELECT * FROM Table A<br/>RIGHT JOIN Table B<br/>ON Table A.id = Table B.id | SELECT * FROM Table A<br/>FULL JOIN Table B<br/>ON Table A.id = Table B.id |



## UNION 与 UNION ALL 合并

- 用于合并两个或多个 SELECT语句 的结果集
- UNION 会选取不同的值（没有重复）
- UNION ALL 会选取全部的值（有重复）

```sql
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2

SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2

// UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
```



## SELECT INTO 备份 / 记录存档

###  表的备份
```sql
SELECT * 
INTO 新表名 [IN 数据库名]
FROM 旧表名
```

###  对记录进行存档
```sql
SELECT 列名
INTO 新表名 [IN 数据库名]
FROM 旧表名
```

## NULL 字段
- 未知的或不适用的值的占位符
- NULL 不等于 0

我们在选择数据的时候，可以使用 IS NULL 或者 IS NOT NULL 选择为空或不为空的数据

##  AUTO INCREMENT 字段
- 每次插入新纪录，自动地创建主键字段的值

- 默认地，开始值为1，每条新纪录递增1；可以自定义开始值

### MySQL 语法
```sql
CREATE TABLE Persons
(
P_Id int NOT NULL AUTO_INCREMENT,
PRIMARY KEY (P_Id)
)

自定义起始值
ALTER TABLE Persons AUTO_INCREMENT=100
```

### SQL Server 语法
```sql
CREATE TABLE Persons
(
P_Id int PRIMARY KEY IDENTITY
)
```

## Constraints 约束

- 限制加入表的数据的类型
- 可以在创建表时加(CREATE TABLE)，也可以创建后加约束(ALTER TABLE)

### NOT NULL
- 强制 列 不接受 NULL 值

```sql
CREATE TABLE Persons
(
id int NOT NULL,
city varchar(255)
)
```

### UNIQUE / PRIMARY KEY / FOREIGN KEY
它们在语法上其实差不多，又都有数据库之间的区别，所以并在一起，方便记忆与对比

- UNIQUE
    - 唯一标识数据库表中的每条记录
    - 与 PRIMARY KEY 约为列或列集合提供了唯一性的保证
    - 每个表可以有多个 UNIQUE 约束，只能有一个 PRIMARY KEY 约束
- PRIMARY KEY
    - 拥有自动定义的 UNIQUE 约束
    - 主键必须包含唯一的值
    - 主键列不能包含NULL值
    - 每个表必须有且只能有一个主键
- FOREIGN KEY
    - 外键
    - 一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY
    - 作用
        - 预防破坏表之间连接的动作
        
        - 防止非法数据插入外键列
        
          

|                            | MySQL 语法                                                   | SQL / Oracle / MS Access                                     |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建（不同）               | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>UNIQUE (Id_P)  // 标识<br/><br/>PRIMARY KEY (id_P)  // 主键<br/><br/>PRIMARY KEY (Id_O),  // 外键<br/>FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)<br/>) | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL UNIQUE  // 标识<br/><br/>Id_P int NOT NULL PRIMARY KEY  // 主键<br/><br/>Id_O int NOT NULL PRIMARY KEY,  // 外键<br/>Id_P int NOT NULL FOREIGN KEY REFERENCES Persons(Id_P)<br/>) |
| 修改                       | ALTER TABLE Persons<br/>ADD UNIQUE (Id_P)  // 标识<br/><br/>ADD PRIMARY KEY (Id_P)  // 主键<br/><br/>ADD FOREIGN KEY (Id_P)  // 外键<br/>REFERENCES Persons(Id_P) |                                                              |
| 撤销                       | ALTER TABLE Orders<br/>DROP INDEX uc_PersonID  // 标识<br/><br/>DROP PRIMARY KEY  //主键<br/><br/>DROP FOREIGN KEY fk_PerOrders  // 外键 |                                                              |
| 创建：多个列的约束，并命名 | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>LastName varchar(255) NOT NULL,<br/>CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)  // 标识<br/><br/>CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)  // 主键<br/><br/>PRIMARY KEY (Id_O),  // 外键<br/>CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)<br/>REFERENCES Persons(Id_P)<br/>) |                                                              |
| 修改：多个列的约束，并命名 | ALTER TABLE Persons<br/>ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,lastName)  // 标识<br/><br/>ADD CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,lastName)  // 主键<br/><br/>ADD CONSTRAINT fk_PerOrders  // 外键<br/>FOREIGN KEY (Id_P)<br/>REFERENCES Persons(Id_P) |                                                              |
| 撤销：多个列的约束，并命名 | ALTER TABLE Orders<br/>DROP CONSTRAINT uc_PersonID  // 标识<br/><br/>DROP CONSTRAINT PRIMARY KEY  //主键<br/><br/>DROP CONSTRAINT fk_PerOrders  // 外键 |                                                              |

### CHECK
- 限制列中的值的范围

|      | MySQL                                                        | SQL / Oracle / MS Access                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建 | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>CHECK (Id_P>0)<br/>)<br/><br/>CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>City varchar(255),<br/>CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')<br/>) | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL CHECK (Id_P>0),<br/>)<br/><br/>CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>City varchar(255),<br/>CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')<br/>) |
| 修改 | ALTER TABLE Persons<br/>ADD CHECK (Id_P>0)<br/><br/>ALTER TABLE Persons<br/>ADD CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes') |                                                              |
| 撤销 | ALTER TABLE Persons<br/>DROP CHECK chk_Person                | ALTER TABLE Persons<br/>DROP CONSTRAINT chk_Person           |

### DEFAULT 默认值
- 插入默认值

|      | MySQL                                                        | SQL / Oracle / MS Access                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建 | CREATE TABLE Persons<br/>(<br/>Id_P int NOT NULL,<br/>City varchar(255) DEFAULT 'Sandnes'<br/>) |                                                              |
| 修改 | ALTER TABLE Persons<br/>ALTER City SET DEFAULT 'SANDNES'     | ALTER TABLE Persons<br/>ALTER COLUMN City SET DEFAULT 'SANDNES' |
| 撤销 | ALTER TABLE Persons<br/>ALTER City DROP DEFAULT              | ALTER TABLE Persons<br/>ALTER COLUMN City DROP DEFAULT       |



## 服务器

- DBMS - 数据库管理系统（Database Management System）
- RDBMS - 关系数据库管理系统（Relational Database Management System）

### 区别
| DBMS                                                         | RDBMS                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 将数据存储为文件，容易出现数据冗余                           | 将数据存储在表中，每个表中有一个主键作为唯一标识符           |
| 数据以导航或分层形式存储，每个数据单元通常具有父节点和一个或多个子节点。对于大量复杂的数据，访问数据速度较慢 | 数据之间的关系以表格形式存储，所以数据更容易地访问，存储在系统中的数据在逻辑上和物理上也是独立的。 |
| 不支持                                                       | 支持数据完整性、高级查询能力、数据库安全性和结构独立性       |
|                                                              | 是DBMS的增强版本；RDBMS 是 SQL 的基础，现代的 SQL 服务器构建在 RDBMS 之上 |
