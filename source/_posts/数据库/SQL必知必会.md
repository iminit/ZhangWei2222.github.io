---
title: SQL必知必会
date: 2018.11.17 18:10:12
categories:
- 数据库
tags:
- SQL
comments: false
---

```
graph LR
A-->B
```
1.1 数据库基础
数据库（database）
保存有组织的数据的容器（通常是一个文件或一组文件）。

表（table）
某种特定类型数据的结构化清单。

模式
关于数据库和表的布局及特性的信息。

列（column）
表中的一个字段。所有表都是由一个或多个列组成的。

行（row）
表中的一个记录。

主键（primary key）
一列（或一组列），其值能够唯一标识表中每一行。

1.2
SQL
SQL（发音为字母S-Q-L或sequel）是结构化查询语言（Structured Query Language）的缩写。SQL是一种专门用来与数据库沟通的语言。

第 2 章 MySQL简介
2.1 什么是MySQL
MySQL是一种DBMS（数据库管理系统）

2.1.1 客户机—服务器软件
DBMS可分为两类：一类为基于共享文件系统的DBMS，另一类为基于客户机—服务器的DBMS。(MySQL)

服务器部分是负责所有数据访问和处理的一个软件。这个软件运行在称为数据库服务器的计算机上。关于数据、数据添加、删除
和数据更新的所有请求都由服务器软件完成。
这些请求或更改来自运行客户机软件的计算机。客户机是与用户打交道的软件。

2.2 MySQL工具
为了使用MySQL，需要有一个客户机，即你需要用来与MySQL打交道（给MySQL提供要执行的命令）的一个应用。

2.2.1 mysql命令行实用程序

在操作系统命令提示符下输入mysql
会出现welcome to the mysql monitor

第３章 使用MySQL 
3.1 连接
为了连接到MySQL，需要以下信息：
 主机名（计算机名）——如果连接到本地MySQL服务器，为localhost；
 端口（如果使用默认端口3306之外的端口）;
 一个合法的用户名；
 用户口令（如果需要）。

3.2 选择数据库
输入 use crashcourse;
输出 Database changed
分析 USE语句并不返回任何结果。依赖于使用的客户机，显示某种形式的通知。例如，这里显示出的Database changed消息是
mysql命令行实用程序在数据库选择成功后显示的。
（use为一个关键字）

3.3 了解数据库和表
1.SHOW DATABASES;返回可用数据库的一个列表。包含在这个列
表中的可能是MySQL内部使用的数据库（如例子中的mysql和information_schema）。当然，你自己的数据库列表可能看上去与这里的不一样。

2.SHOW TABLES;返回当前选择的数据库内可用表的列表。SHOW也可以用来显示表列：

3.SHOW COLUMNS 要求给出一个表名（这个例子中的 FROM customers），它对每个字段返回一行，行中包含字段名、数据
类型、是否允许NULL、键信息、默认值以及其他信息（如字段cust_id的auto_increment）。
(DESCRIBE customers == SHOW COLUMNS FROM customers;)

4.SHOW STATUS，用于显示广泛的服务器状态信息；

5.SHOW CREATE DATABASE和SHOW CREATE TABLE，分别用来显示创建特定数据库或表的MySQL语句；

6.SHOW GRANTS，用来显示授予用户（所有用户或特定用户）的安全权限；

7.SHOW ERRORS和SHOW WARNINGS，用来显示服务器错误或警告消息。

第 4 章  检 索 数 据

4.1 SELECT语句
为了使用SELECT检索表数据，必须至少给出两条信息——想选择什
么，以及从什么地方选择。

4.2 检索单个列
SELECT prod_name
FROM products;

上述语句利用SELECT语句从products表中检索一个名为prod_name的列。所需的列名在SELECT关键字之后给出，FROM关键字指出从其中检索数据的表名。

4.3 检索多个列
SELECT prod_name,prod_id
FROM products;

4.4 检索所有列
SELECT *
FROM products;

4.5 检索不同的行
SELECT vend_id
FROM products;

会返回所有匹配的行，但是不想每个值每次都出现。比如上述命令，返回了14个行，因为有14个产品，但是只有4个经销商

SELECT DISTINCT vend_id
FROM products;

(不能部分使用DISTINCT DISTINCT关键字应用于所有列而
不仅是前置它的列。如果给出SELECT DISTINCT vend_id,
prod_price，除非指定的两个列都不同，否则所有行都将被
检索出来。)

4.6 限制结果
SELECT prod_name
FROM products
LIMIT 5;
返回不多于5行

SELECT prod_name
FROM products
LIMIT 5,5;
从第5行开始的5行，两个参数时，第一个为开始的位置，第二个为要检索的行数。

注释：
1.返回的数据没有顺序可言。
2.多条SQL语句必须以分号（；）分隔，不需要在单条SQL语句后加分号。如果你使用的是mysql命令行，必须加上分号来结束SQL语句。
3.SQL语句不区分大小写，建议对所有SQL关键字使用大写，而对所有列和表名使用小写。
4.在处理SQL语句时，其中所有空格都被忽略。SQL语句可以在一行上给出，也可以分成许多行。分开来比较易于读

第５章 排序检索数据

5.1 排序数据

SELECT prod_name
FROM products
ORDER BY prod_name;

(子句（clause）
一个子句通常由一个关键字和所提供的数据组
成。子句的例子有SELECT语句的FROM子句。)

5.2 按多个列排序

SELECT prod_name,prod_price
FROM products
ORDER BY prod_name,prod_price;

5.3 指定排序方向

SELECT pord_id,prod_name,prod_price
FROM products
ORDER BY prod_name DESC,prod_price;

DESC关键字只应用到直接位于其前面的列名.降序

第６章 过 滤 数 据

6.1 使用WHERE子句

SELECT prod_name,prod_price
FROM products
WHERE prod_price = 2.50;

从products表中检索两个列，但不返回所有行，只返
回prod_price值为2.50的行

(在同时使用ORDER BY和WHERE子句时，应
该让ORDER BY位于WHERE之后，否则将会产生错误)

6.2 WHERE子句操作符

6.2.1 检查单个值
SELECT prod_name,prod_price
FROM products
WHERE prod_name = 'wei';

MySQL在执行匹配时默认不区分大小写，所
以wei与WEI匹配。

6.2.2 不匹配检查

6.2.3 范围值检查

SELECT prod_name,prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;

在使用BETWEEN时，必须指定两个值
——所需范围的低端值和高端值。这两个值必须用AND关键字
分隔。BETWEEN匹配范围中所有的值，包括指定的开始值和结束值。

6.2.4 空值检查

(NULL 无值（no value），它与字段包含0、空字符串或仅仅包含
空格不同。)

SELECT prod_name,prod_price
FROM products
WHERE prod_price IS NULL;

第 7 章 数 据 过 滤

7.1 组合WHERE子句

(操作符（operator） 用来联结或改变WHERE子句中的子句的关键
字。也称为逻辑操作符（logical operator）。)

7.1.1 AND操作符

SELECT pord_id,prod_name,prod_price
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;

7.1.2 OR操作符

SELECT prod_name,prod_price
FROM products
WHERE vend_id = 1002 OR vend_id = 1003;

7.1.3 计算次序

WHERE可包含任意数目的AND和OR操作符。允许两者结合以进行复杂
和高级的过滤。AND的优先度大于OR，所以一定要用圆括号

任何时候使用具有AND和OR操作
符的WHERE子句，都应该使用圆括号明确地分组操作符。

SELECT prod_name,prod_price
FROM products
WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 13;

7.2 IN操作符

SELECT prod_name,prod_price
FROM products
WHERE vend_id IN (1002，1003);

IN WHERE子句中用来指定要匹配值的清单的关键字，功能与OR
相当。用IN，更好

7.3 NOT操作符

否定它之后所
跟的任何条件。

SELECT prod_name,prod_price
FROM products
WHERE vend_id NOT IN (1002，1003);

第 8 章 用通配符进行过滤

8.1 LIKE操作符 

怎样搜索产品名中包含文本anvil的所有产品？用简单
的比较操作符肯定不行，必须使用通配符。利用通配符可创建比较特定
数据的搜索模式。在这个例子中，如果你想找出名称包含anvil的所有产
品，可构造一个通配符搜索模式，找出产品名中任何位置出现anvil的产
品。

通配符（wildcard） 用来匹配值的一部分的特殊字符。
搜索模式（search pattern）① 由字面值、通配符或两者组合构
成的搜索条件。

8.1.1 百分号（%）通配符

在搜索串中，%表示任何字符出现
任意次数。

SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE 'jet%';

此例子使用了搜索模式'jet%'。在执行这条子句时，将检索任
意以jet起头的词。%告诉MySQL接受jet之后的任意字符，不
管它有多少字符。

也可以放到开头，%jet%

(注意尾空格 :在保存词
anvil 时，如果它后面有一个或多个空格，则子句WHERE
prod_name LIKE '%anvil'将不会匹配它们，因为在最后的l
后有多余的字符。解决这个问题的一个简单的办法是在搜索模
式最后附加一个%。一个更好的办法是使用函数（第11章将会
介绍）去掉首尾空格。)

(注意NULL 虽然似乎%通配符可以匹配任何东西，但有一个例
外，即NULL。不匹配NULL)

8.1.2 下划线（_）通配符

下划线的用途与%一样，但下划
线只匹配单个字符而不是多个字符。

SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE '_ ton anvil';

只能匹配 1 ton anvil,不能.5 ton anvil

8.2 使用通配符的技巧

不要过度使用通配符。

第9章 用正则表达式进行搜索

9.2.1 基本字符匹配

LIKE与REGEXP 在LIKE和REGEXP之间有一个重要的差别。请
看以下两条语句：

SELECT prod_name
FROM products
WHERE prod_name LIKE '1000';

SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000';

如果执行上述两条语句，会发现第一条语句不返回数据，而第
二条语句返回一行。为什么？
正如第8章所述，LIKE匹配整个列。如果被匹配的文本在列值
中出现，LIKE将不会找到它，相应的行也不被返回（除非使用
通配符）。而REGEXP在列值内进行匹配，如果被匹配的文本在
列值中出现，REGEXP将会找到它，相应的行将被返回。这是一
个非常重要的差别。
那么，REGEXP能不能用来匹配整个列值（从而起与LIKE相同
输入
输出
分析
的作用）？答案是肯定的，使用^和$定位符（anchor）即可，
本章后面介绍。

SELECT prod_name
FROM products
WHERE prod_name REGEXP '.1000';

.是正则表达式语言中一个特殊
的字符。它表示匹配任意一个字符，

(匹配不区分大小写 MySQL中的正则表达式匹配（自版本
3.23.4后）不区分大小写（即，大写和小写都匹配）。为区分大
小写，可使用BINARY关键字，如WHERE prod_name REGEXP
BINARY 'JetPack .000'。)

9.2.2 进行OR匹配

SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000';

9.2.3 匹配几个字符之一

SELECT prod_name
FROM products
WHERE prod_name REGEXP '[123] ton';

[123] = [1|2|3]

[^123]匹配除这些字符外的任何东西。

9.2.4 匹配范围

集合可用来定义要匹配的一个或多个字符。[1-3] [1-9] [a-z]

9.2.5 匹配特殊字符

为了匹配特殊字符，必须用\\为前导。\\-表示查找-，\\.表示查找.。这种处理
就是所谓的转义.

\\f 换页
\\n 换行
\\r 回车
\\t 制表
\\v 纵向制表

9.2.6 匹配字符类

存在找出你自己经常使用的数字、所有字母字符或所有数字字母字
符等的匹配。为更方便工作，可以使用预定义的字符集，称为字符类

类         说明
[:alnum:] 任意字母和数字（同[a-zA-Z0-9]）
[:alpha:] 任意字符（同[a-zA-Z]）
[:blank:] 空格和制表（同[\\t]）
[:cntrl:] ASCII控制字符（ASCII 0到31和127）
[:digit:] 任意数字（同[0-9]）
[:graph:] 与[:print:]相同，但不包括空格
[:lower:] 任意小写字母（同[a-z]）
[:print:] 任意可打印字符
[:punct:] 既不在[:alnum:]又不在[:cntrl:]中的任意字符
[:space:] 包括空格在内的任意空白字符（同[\\f\\n\\r\\t\\v]）
[:upper:] 任意大写字母（同[A-Z]）
[:xdigit:] 任意十六进制数字（同[a-fA-F0-9]）

9.2.7 匹配多个实例

重复元字符

元字符 说 明
* 0个或多个匹配
+ 1个或多个匹配（等于{1,}）
? 0个或1个匹配（等于{0,1}）
{n} 指定数目的匹配
{n,} 不少于指定数目的匹配
{n,m} 匹配数目的范围（m不超过255）

SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
ORDER BY prod_name;

正则表达式\\([0-9] sticks?\\)需要解说一下。\\(匹配)，
[0-9]匹配任意数字（这个例子中为1和5），sticks?匹配stick
和sticks（s后的?使s可选，因为?匹配它前面的任何字符的0次或1次出
现），\\)匹配)。没有?，匹配stick和sticks会非常困难。

SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'
ORDER BY prod_name;

，[:digit:]匹配任意数字，因而它为数字的一个集
合。{4}确切地要求它前面的字符（任意数字）出现4次，所以
[[:digit:]]{4}匹配连在一起的任意4位数字。

9.2.8 定位符

定位元字符
元字符 说 明
^ 文本的开始
$ 文本的结尾
[[:<:]] 词的开始
[[:>:]] 词的结尾

SELECT prod_name
FROM products
WHERE prod_name REGEXP '^[0-9\\.]'
ORDER BY prod_name;

^匹配串的开始。因此，^[0-9\\.]只在.或任意数字为串中第
一个字符时才匹配它们。没有^，则还要多检索出4个别的行（那
些中间有数字的行）。

(^的双重用途 ^有两种用法。在集合中（用[和]定义），用它
来否定该集合，否则，用来指串的开始处。)

第 10 章  创建计算字段

10.1 计算字段

计算字段发挥作用:存储在表中的数据都不是应用程序所需要的。
我们需要直接从数据库中检索出转换、计算或格式化过的数据；而不是
检索出数据，然后再在客户机应用程序或报告程序中重新格式化。计算字段并不实际存在于数据库表中。计算字段是运行时在SELECT语句
内创建的。

(字段（field） 基本上与列（column）的意思相同，经常互换使
用)

10.2 拼接字段

拼接（concatenate） 将值联结到一起构成单个值。

SELECT Concat(vend_name,'(',vend_country,')')
FROM vendors
ORDER BY vend_name;

Concat()拼接串，即把多个串连接起来形成一个较长的串。
Concat()需要一个或多个指定的串，各个串之间用逗号分隔。

SELECT Concat(RTrim(vend_name),'(',RTrim(vend_country),')')
FROM vendors
ORDER BY vend_name;

RTrim()函数去掉值右边的所有空格.LTrim() Trim()

使用别名

SELECT Concat(RTrim(vend_name),'(',RTrim(vend_country),')') AS vend_title
FROM vendors
ORDER BY vend_name;

10.3 执行算术计算

SELECT prod_id,quantity,item_price
FROM orderitems
WHERE order_num = 2005;

计算字段的另一常见用途是对检索出的数据进行算术计算。举一个
例子，orders表包含收到的所有订单，orderitems表包含每个订单中的
各项物品。

SELECT prod_id,
quantity,
item_price,
quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 2005;

第 11 章 使用数据处理函数

11.2.1 文本处理函数

常用的文本处理函数

函 数 说 明
Left() 返回串左边的字符
Length() 返回串的长度
Locate() 找出串的一个子串
Lower() 将串转换为小写
LTrim() 去掉串左边的空格
Right() 返回串右边的字符
RTrim() 去掉串右边的空格
Soundex() 返回串的SOUNDEX值
SubString() 返回子串的字符
Upper() 将串转换为大写

SOUNDEX是一个将任何文
本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似
的发音字符和音节，使得能对串进行发音比较而不是字母比较。

SELECT cust_name,cust_contact
FROM customers
WHERE Soundex(cust_contact) = Soundex('Y Lie');

WHERE子句使用Soundex()函数来转换cust_
contact列值和搜索串为它们的SOUNDEX值。因为Y.Lee和
Y.Lie发音相似，所以它们的SOUNDEX值匹配

11.2.2 日期和时间处理函数

常用日期和时间处理函数

函 数 说 明
AddDate() 增加一个日期（天、周等）
AddTime() 增加一个时间（时、分等）
CurDate() 返回当前日期
CurTime() 返回当前时间
Date() 返回日期时间的日期部分
DateDiff() 计算两个日期之差
Date_Add() 高度灵活的日期运算函数
Date_Format() 返回一个格式化的日期或时间串
Day() 返回一个日期的天数部分
DayOfWeek() 对于一个日期，返回对应的星期几
Hour() 返回一个时间的小时部分
Minute() 返回一个时间的分钟部分
Month() 返回一个日期的月份部分
Now() 返回当前日期和时间
Second() 返回一个时间的秒部分
Time() 返回一个日期时间的时间部分
Year() 返回一个日期的年份部分

首先需要注意的是MySQL使用的日期格式。无论你什么时候指定一
93
图灵社区会员 臭豆腐(StinkBC@gmail.com) 专享 尊重版权
72 第 11 章 使用数据处理函数
个日期，不管是插入或更新表值还是用WHERE子句进行过滤，日期必须为
格式yyyy-mm-dd。因此，2005年9月1日，给出为2005-09-01。

SELECT cust_id,order_num
FROM orders
WHERE Date(order_date) = '2015-09-01';

SELECT cust_id,order_num
FROM orders
WHERE Year(order_date) = 2015 AND Month(order_date)= 9;

11.2.3 数值处理函数

常用数值处理函数

函 数 说 明
Abs() 返回一个数的绝对值
Cos() 返回一个角度的余弦
Exp() 返回一个数的指数值
Mod() 返回除操作的余数
Pi() 返回圆周率
Rand() 返回一个随机数
Sin() 返回一个角度的正弦
Sqrt() 返回一个数的平方根
Tan() 返回一个角度的正切

第 12 章 汇 总 数 据

SQL聚集函数

函 数 说 明
AVG() 返回某列的平均值
COUNT() 返回某列的行数
MAX() 返回某列的最大值
MIN() 返回某列的最小值
SUM() 返回某列值之和

SELECT COUNT(*) AS num_items,
       MIN(prod_price) AS price_min,
       MAX(prod_price) AS price_max,
       AVG(prod_price) AS price_avg
FROM products;

1.为了获得多个列的平均值，
必须使用多个AVG()函数。AVG()函数忽略列值为NULL的行。
2.如果指定列名，则指定列的值为空的行被COUNT()
函数忽略，但如果COUNT()函数中用的是星号（*），则不忽
略。
3.以上5个聚集函数都可以如下使用： 对所有的行执行计算，指定ALL参数或不给参数（因为ALL是默认
行为）；
 只包含不同的值，指定DISTINCT参数。
如果指定列名，则DISTINCT只能用于COUNT()。DISTINCT
不能用于COUNT(*)，因此不允许使用COUNT（DISTINCT），
否则会产生错误。类似地，DISTINCT必须使用列名，不能用
于计算或表达式。

SELECT AVG(DISTINCT prod_price) AS price_avg
FROM products
WHERE vend_id = 1003;

4.取别名

第 13 章 分 组 数 据
13.1 数据分组
13.2 创建分组
分组是在SELECT语句的GROUP BY子句中建立的。

SELECT vend_id,COUNT(*) AS num_prods
FROM products
GROUP BY vend_id;

vend_id num_prods
1001 3
1002 2
1003 7
1005 2

上面的SELECT语句指定了两个列，vend_id包含产品供应商的ID，
num_prods为计算字段（用COUNT(*)函数建立）。GROUP BY子句指
示MySQL按vend_id排序并分组数据。这导致对每个vend_id而不是整个表
计算num_prods一次。从输出中可以看到，供应商1001有3个产品，供应商
1002有2个产品，供应商1003有7个产品，而供应商1005有2个产品。

重要的规定
1.GROUP BY子句可以包含任意数目的列
2.在建立分组时，指定的所有列都一起计算
（所以不能从个别的列取回数据）。
3.如果在SELECT中使用表达式，则必须在
GROUP BY子句中指定相同的表达式。不能使用别名。
4.除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子
句中给出。
5.如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列
中有多行NULL值，它们将分为一组。
6.GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

使用ROLLUP 使用WITH ROLLUP关键字，可以得到每个分组以
及每个分组汇总级别（针对每个分组）的值，如下所示：

SELECT vend_id,COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP;

13.3 过滤分组
目前为止所
学过的所有类型的WHERE子句都可以用HAVING来替代。唯一的差别是
WHERE过滤行，而HAVING过滤分组。

SELECT vend_id,COUNT(*) AS num_prods
FROM products
GROUP BY vend_id
HAVING COUNT(*) >= 2;

最后一行增加了
HAVING子句，它过滤COUNT(*) >=2（两个以上的订单）的那些
分组。

HAVING 和 WHERE 结合的例子

SELECT vend_id,COUNT(*) AS num_prods
FROM products
WHERE prod_price >= 10
GROUP BY vend_id
HAVING COUNT(*) >= 2;

13.4 分组和排序
ORDER BY与GROUP BY
ORDER BY GROUP BY 
排序产生的输出 分组行。但输出可能不是分组的顺序
任意列都可以使用（甚至
非选择的列也可以使用）
只可能使用选择列或表达式列，而且必须使用每个选择
列表达式
不一定需要 如果与聚集函数一起使用列（或表达式），则必须使用

不要忘记ORDER BY 一般在使用GROUP BY子句时，应该也给
出ORDER BY子句。这是保证数据正确排序的唯一方法。千万
不要仅依赖GROUP BY排序数据。

SELECT order_num,SUM(quantity*item_price) AS ordertotal
FROM orderitems
GROUP BY order_num
HAVING SUM(quantity*item_price) >= 50
ORDER BY ordertotal;

13.5 SELECT子句顺序
子 句 说 明 是否必须使用
SELECT 要返回的列或表达式 是
FROM 从中检索数据的表 仅在从表选择数据时使用
WHERE 行级过滤 否
GROUP BY 分组说明 仅在按组计算聚集时使用
HAVING 组级过滤 否
ORDER BY 输出排序顺序 否
LIMIT 要检索的行数 否

第 14 章 使用子查询

子查询最常见的使用
是在WHERE子句的IN操作符中，以及用来填充计算列

14.2 利用子查询进行过滤

SELECT cust_id
FROM orders
WHERE order_num IN (SELECT order_num 
                    FROM orderitems
                    WHERE prod_id = 'TBT2')

在SELECT语句中，子查询总是从内向外处理。在处理上面的
SELECT语句时，MySQL实际上执行了两个操作。
首先，它执行下面的查询：SELECT order_num 
                    FROM orderitems
                    WHERE prod_id = 'TBT2'

此查询返回两个订单号：20005和20007。然后，这两个值以IN操作符要
求的逗号分隔的格式传递给外部查询的WHERE子句。外部查询变成：

SELECT cust_id
FROM orders
WHERE order_num IN (20005,20007)

14.3 作为计算字段使用子查询

假如需要显示customers
表中每个客户的订单总数。订单与相应的客户ID存储在orders表中。
为了执行这个操作，遵循下面的步骤。
(1) 从customers表中检索客户列表。
(2) 对于检索出的每个客户，统计其在orders表中的订单数目。

SELECT cust_name,
       cust_state,
       (SELECT COUNT(*)
        FROM orders
        WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;

HERE orders.cust_id = customers.cust_id) AS orders 
用了完全限定列名

相关子查询（correlated subquery） 涉及外部查询的子查询。

第 15 章 联 结 表

15.1.1 关系表

假如有由同一供应商生产的多种物品，那么在何处存储供应
商信息（如，供应商名、地址、联系方法等）呢？

在这个例子中，可建立两个表，一个存储供应商信息，另一个存储
产品信息。vendors表包含所有供应商信息，每个供应商占一行，每个供
应商具有唯一的标识。此标识称为主键（primary key）（在第1章中首次
提到），可以是供应商ID或任何其他唯一值。
products表只存储产品信息，它除了存储供应商ID（vendors表的主
键）外不存储其他供应商信息。vendors表的主键又叫作products的外键，
它将vendors表与products表关联，利用供应商ID能从vendors表中找出
相应供应商的详细信息。

外键（foreign key） 外键为某个表中的一列，它包含另一个表
的主键值，定义了两个表之间的关系。

15.2 创建联结

联结的创建非常简单，规定要联结的所有表以及它们如何关联即可。

SELECT vend_name,prod_name,prod_price
FROM vendors,products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name,prod_name;

1.必须WHERE
2.完全限定列名

笛卡儿积（cartesian product） 由没有联结条件的表关系返回
的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘
以第二个表中的行数。

SELECT vend_name,prod_name,prod_price
FROM vendors,products
ORDER BY vend_name,prod_name;

从上面的输出中可以看到，相应的笛卡儿积不是我们所想要
的。这里返回的数据用每个供应商匹配了每个产品，它包括了
供应商不正确的产品。实际上有的供应商根本就没有产品。

15.2.2 内部联结

目前为止所用的联结称为等值联结（equijoin），它基于两个表之间的
相等测试。这种联结也称为内部联结。

SELECT vend_name,prod_name,prod_price
FROM vendors INNER JOIN products
ON   vendors.vend_id = products.vend_id;

15.2.3 联结多个表

以14章的例子

SELECT cust_name,cust_contact
FROM customers,orders,orderitems
WHERE customers.cust_id = orders.cust_id
AND orderitems.order_num = orders.order_num
AND prod_id = 'TNT2';

第 16 章 创建高级联结

16.1 使用表别名

SELECT cust_name,cust_contact
FROM customers AS c,orders AS O,orderitems AS oi
WHERE c.cust_id = o.cust_id
AND oi.order_num = o.order_num
AND prod_id = 'TNT2';

16.2 使用不同类型的联结

现在来看3种其他联结，它们分别是自联结、自然联结和外部联结。

16.2.1 自联结

假如你发现某物品（其ID为DTNTR）存在问题，因此想知道生产该物
品的供应商生产的其他物品是否也存在这些问题。

SELECT prod_id,prod_name
FROM products
WHERE vend_id = (SELECT vend_id
                 FROM products
                 WHERE prod_id = 'DTNTR');
        
SELECT p1.prod_id,p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
AND p2.prod_id = 'DTNTR';

用自联结而不用子查询 自联结通常作为外部语句用来替代
从相同表中检索数据时使用的子查询语句。虽然最终的结果是
相同的，但有时候处理联结远比处理子查询快得多。应该试一
下两种方法，以确定哪一种的性能更好

16.2.2 自然联结

无论何时对表进行联结，应该至少有一个列出现在不止一个表中（被
联结的列）。标准的联结（前一章中介绍的内部联结）返回所有数据，甚
至相同的列多次出现。自然联结排除多次出现，使每个列只返回一次。
这一
般是通过对表使用通配符（SELECT *），对所有其他表的列使用明确的子
集来完成的。

SELECT c.*,o.order_num,o.order_date,oi.prod_id,oi.quantity,oi.item_price
FROM customers AS c,orders AS o,orderitems AS oi
WHERE c.cust_id = o.cust_id
AND oi.order_num = o.order_num
AND prod_id = 'FB';

事实上，迄今为止我们建立的每个内部联结都是自然联结，很可能
我们永远都不会用到不是自然联结的内部联结

16.2.3 外部联结

联结包含了那些在相关表中没有关联行的行。这种
类型的联结称为外部联结。

为了检索所有客户，包括那些没有订单的客户，
可如下进行

SELECT customers.cust_id,orders.order_num
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id;

这条SELECT语句使用了关
键字OUTER JOIN来指定联结的类型（而不是在WHERE子句中指
定）。但是，与内部联结关联两个表中的行不同的是，外部联结还包括没
有关联行的行。在使用OUTER JOIN语法时，必须使用RIGHT或LEFT关键字
指定包括其所有行的表（RIGHT指出的是OUTER JOIN右边的表，而LEFT
指出的是OUTER JOIN左边的表）。上面的例子使用LEFT OUTER JOIN从FROM
子句的左边表（customers表）中选择所有行。

16.3 使用带聚集函数的联结

如果要检索所有客户及每个客户所
下的订单数

SELECT customers.cust_name,
       customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;

此SELECT语句使用INNER JOIN将customers和orders表互相关联。
GROUP BY 子句按客户分组数据，因此，函数调用 COUNT 
(orders.order_num)对每个客户的订单计数，将它作为num_ord返回。

第 17 章 组 合 查 询

17.1 组合查询

也允许执行多个查询（多条SELECT语句），并将结果作为单个
查询结果集返回。这些组合查询通常称为并（union）或复合查询
（compound query）。
有两种基本情况，其中需要使用组合查询：
 在单个查询中从不同的表返回类似结构的数据；
 对单个表执行多个查询，按单个查询返回数据。

组合查询和多个WHERE条件 多数情况下，组合相同表的两个
查询完成的工作与具有多个WHERE子句条件的单条查询完成的
工作相同。换句话说，任何具有多个WHERE子句的SELECT语句
都可以作为一个组合查询给出

17.2 创建组合查询
17.2.1 使用UNION
假如需要价格小于等于5的所有物品的一个列表，而且
还想包括供应商1001和1002生产的所有物品（不考虑价格）

SELECT vend_id,prod_id,prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id,prod_id,prod_price
FROM products
WHERE cend_id IN (1001,1002);

由前面的两条SELECT语句组成，语句中用UNION关键
字分隔。UNION指示MySQL执行两条SELECT语句，并把输出组
合成单个查询结果集。
作为参考，这里给出使用多条WHERE子句而不是使用UNION的相同查询：

SELECT vend_id,prod_id,prod_price
FROM products
WHERE prod_price <= 5
   OR vend_id IN (1001,1002);

17.2.2 UNION规则

 UNION必须由两条或两条以上的SELECT语句组成，语句之间用关
键字UNION分隔（因此，如果组合4条SELECT语句，将要使用3个
UNION关键字）。
 UNION中的每个查询必须包含相同的列、表达式或聚集函数（不过
各个列不需要以相同的次序列出）。
 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以
隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。

17.2.3 包含或取消重复的行

UNION从查询结果集中自动去除了重复的行（换句话说，它的行为与
单条SELECT语句中使用多个WHERE子句条件一样）。这是UNION的默认行为，但是如果需要，可以改变它。事实上，如果
想返回所有匹配行，可使用UNION ALL而不是UNION。

17.2.4 对组合查询结果排序

在用UNION组合查询时，只
能使用一条ORDER BY子句，它必须出现在最后一条SELECT语句之后。

SELECT vend_id,prod_id,prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id,prod_id,prod_price
FROM products
WHERE cend_id IN (1001,1002)
ORDER BY vend_id,prod_price;

第 18 章 全文本搜索

18.1 理解全文本搜索

并非所有引擎都支持全文本搜索:两个最常使用的引擎为MyISAM和InnoDB，
前者支持全文本搜索，而后者不支持。

18.2.1 启用全文本搜索支持

一般在创建表时启用全文本搜索。

CREATE TABLE productnotes
(
  note_id int NOT NULL AUTO_INCREMENT,
  prod_id char(10) NOT NULL,
  note_date datetime NOT NULL,
  note_text text NULL,
  PRIMARY KEY(node_id),
  FULLTEXT(node_text)
) ENGING=MyISAM;

这条
CREATE TABLE语句定义表productnotes并列出它所包含的列,这些列中有一个名为note_text的列，为了进行全文本搜索，
MySQL根据子句FULLTEXT(note_text)的指示对它进行索引。这里的
FULLTEXT索引单个列，如果需要也可以指定多个列。

在定义之后，MySQL自动维护该索引。在增加、更新或删除行时，
索引随之自动更新。

不要在导入数据时使用FULLTEXT

18.2.2 进行全文本搜索

在索引之后，使用两个函数Match()和Against()执行全文本搜索，
其中Match()指定被搜索的列，Against()指定要使用的搜索表达式。

SELECT node_text
FROM productnotes
WHERE Match(note_text) Aginst('rabbit');

1.使用完整的 Match() 说 明 传递给 Match() 的值必须与
FULLTEXT()定义中的相同。如果指定多个列，则必须列出它
们（而且次序正确）。
2.搜索不区分大小写

18.2.3 使用查询扩展

查询扩展用来设法放宽所返回的全文本搜索结果的范围。例如：你想找出所有提到anvils的注释。只有一个注释包含词anvils，
但你还想找出可能与你的搜索有关的所有其他行，即使它们不包含词anvils。

在使用查询扩展时，MySQL对数据和
索引进行两遍扫描来完成搜索： 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有
行；
 其次，MySQL检查这些匹配行并选择所有有用的词（我们将会简
要地解释MySQL如何断定什么有用，什么无用）。
 再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件，
而且还使用所有有用的词。

SELECT node_text
FROM productnotes
WHERE Match(note_text) Aginst('anvils' WITH QUERY EXPANSION);

18.2.4 布尔文本搜索

MySQL支持全文本搜索的另外一种形式，称为布尔方式（boolean
mode）。

即使没有FULLTEXT索引也可以使用,但这是一种非常缓慢的操作
（其性能将随着数据量的增加而降低）。

SELECT node_text
FROM productnotes
WHERE Match(note_text) Aginst('heavy' IN BOOLEAN MOOE);

SELECT node_text
FROM productnotes
WHERE Match(note_text) Aginst('heavy -rope*' IN BOOLEAN MOOE);

匹配包含heavy但不包含任意以rope开始的词的行.

全文本布尔操作符
布尔操作符 说 明
+ 包含，词必须存在
- 排除，词必须不出现
> 包含，而且增加等级值
< 包含，且减少等级值
() 把词组成子表达式（允许这些子表达式作为一个组被包含、
排除、排列等）
~ 取消一个词的排序值
* 词尾的通配符
"" 定义一个短语（与单个词的列表不一样，它匹配整个短语以
便包含或排除这个短语）

在布尔方式中，不按等级值降序排序返回的
行。

18.2.5 全文本搜索的使用说明

 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为
那些具有3个或3个以下字符的词（如果需要，这个数目可以更改）。
 MySQL带有一个内建的非用词（stopword）列表，这些词在索引
全文本数据时总是被忽略。如果需要，可以覆盖这个列表（请参
阅MySQL文档以了解如何完成此工作）。
 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。
因此，MySQL规定了一条50%规则，如果一个词出现在50%以上
的行中，则将它作为一个非用词忽略。50%规则不用于IN BOOLEAN
MODE。
 如果表中的行数少于3行，则全文本搜索不返回结果（因为每个词
或者不出现，或者至少出现在50%的行中）。
 忽略词中的单引号。例如，don't索引为dont。
 不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文
本搜索结果。
 如前所述，仅在MyISAM数据库引擎中支持全文本搜索。 

第 19 章 插 入 数 据

19.1 数据插入

INSERT是用来插入（或添加）行到数据库表的。插入可
以用几种方式使用：
 插入完整的行；
 插入行的一部分；
 插入多行；
 插入某些查询的结果

19.2 插入完整的行

INSERT INTO customers(cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_country,
  cust_contact,
  cust_email)
VALUES('Pep E. LaPew',
  '100 Main Street',
  'Los Angeles',
  'CA',
  '90046',
  'USA',
  NULL,
  NULL);

  省略列 如果表的定义允许，则可以在INSERT操作中省略某
些列。省略的列必须满足以下某个条件。
 该列定义为允许NULL值（无值或空值）。
 在表定义中给出默认值。这表示如果不给出值，将使用默
认值。

19.3 插入多个行

INSERT INTO customers(cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_country)
VALUES('Pep E. LaPew',
  '100 Main Street',
  'Los Angeles',
  'CA',
  '90046',
  'USA'),
  ('Pep E. LaPew',
  '100 Main Street',
  'Los Angeles',
  'CA',
  '90046',
  'USA'
  );

其中单条INSERT语句有多组值，每组值用一对圆括号括起来，
用逗号分隔。

19.4 插入检索出的数据

想从另一表中合并客户列表到你的customers表,应该首先创建和填
充custnew表。custnew表的结构与附录B中描述的customers
表的相同。在填充custnew时，不应该使用已经在customers
中使用过的cust_id值（如果主键值重复，后续的INSERT操作
将会失败）或仅省略这列值让MySQL在导入数据的过程中产
生新值。

INSERT INTO customers(cust_id,
  cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_country)
SELECT cust_id,
  cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_country
FROM custnew;

第 20 章 更新和删除数据

20.1 更新数据

为了更新（修改）表中的数据，可使用UPDATE语句。

 更新表中特定行；
 更新表中所有行。

基本的
UPDATE语句由3部分组成，分别是：
 要更新的表；
 列名和它们的新值；
 确定要更新行的过滤条件。

客户10005现在有了电子邮件地址:

UPDATE customers
SET cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;

更新多个列时，只需要使用单个SET命令，每个“列=值”对之间
用逗号分隔（最后一列之后不用逗号）。

UPDATE customers
SET cust_name = 'The Fudds',
    cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;

为了删除某个列的值，可设置它为NULL（假如表定义允许NULL值）。

UPDATE customers
SET cust_email = NULL
WHERE cust_id = 10005;

20.2 删除数据

为了从一个表中删除（去掉）数据，使用DELETE语句。可以两种方
式使用DELETE：
 从表中删除特定的行；
 从表中删除所有行。

DELECT FROM customers
WHERE cust_id = 10006;

DELETE FROM要求指定从中删除数据的表名。
WHERE子句过滤要删除的行。在这个例子中，只删除客户10006。如果省
略WHERE子句，它将删除表中每个客户。
DELETE不需要列名或通配符。DELETE删除整行而不是删除列。为了
删除指定的列，请使用UPDATE语句。

删除表的内容而不是表 DELETE语句从表中删除行，甚至是
删除表中所有行。但是，DELETE不删除表本身。

如果想从表中删除所有行，不要使用DELETE。
可使用TRUNCATE TABLE语句

20.3 更新和删除的指导原则

 除非确实打算更新和删除每一行，否则绝对不要使用不带WHERE
子句的UPDATE或DELETE语句。
 保证每个表都有主键（如果忘记这个内容，请参阅第15章），尽可能
像WHERE子句那样使用它（可以指定各主键、多个值或值的范围）。
 在对UPDATE或DELETE语句使用WHERE子句前，应该先用SELECT进
行测试，保证它过滤的是正确的记录，以防编写的WHERE子句不
正确。
 使用强制实施引用完整性的数据库（关于这个内容，请参阅第15
章），这样MySQL将不允许删除具有与其他表相关联的数据的行。

第 21 章 创建和操纵表

21.1 创建表

一般有两种创建表的方法：
 使用具有交互式创建和管理表的工具（如第2章讨论的工具）；
 表也可以直接用MySQL语句操纵。

值得注意的
是，在使用交互式工具时，实际上使用的是MySQL语句。

21.1.1 表创建基础

为利用CREATE TABLE创建表，必须给出下列信息：
 新表的名字，在关键字CREATE TABLE之后给出；
 表列的名字和定义，用逗号分隔。

CREATE TABLE customers
(
    cust_id int NOT NULL AUTO_INCREMENT,
    cust_name char(50) NOT NULL,
    cust_address char(50) NULL,
    cust_city char(50) NULL,
    PRIMARY KEY (cust_id)
) ENGINE = InnoDB;

(处理现有的表 在创建新表时，指定的表名必须不存在，否则
将出错。如果要防止意外覆盖已有的表，SQL要求首先手工删
除该表（请参阅后面的小节），然后再重建它，而不是简单地
用创建表语句覆盖它。)

21.1.2 使用NULL值

在插入或更新行时，该列必须有值。NULL为默认设置，如果不指定NOT NULL，则认为指定
的是NULL。NULL值是没有值，
它不是空串。

21.1.3 主键再介绍

主键值必须唯一。即，表中的每个行必须具有唯一的主
键值。如果主键使用单个列，则它的值必须唯一。如果使用多个列，则
这些列的组合值必须唯一。

CREATE TABLE orderitems
(
    order_num int NOT NULL,
    order_item int NOT NULL,
    prod_id char(10) NOT NULL,
    quantity int NOT NULL,
    item_price decimal(8,2) NOT NULL,
    PRIMARY KEY (order_num,order_item)
) ENGINE=InnoDB;

orderitems表包含orders表中每个订单的细节。每个订单有多项物
品，但每个订单任何时候都只有1个第一项物品，1个第二项物品，如此
等等。因此，订单号（order_num列）和订单物品（order_item列）的组
合是唯一的，从而适合作为主键

21.1.4 使用AUTO_INCREMENT

AUTO_INCREMENT告诉MySQL，本列每当增加一行时自动增量。每次
执行一个INSERT操作时，MySQL自动对该列增量（从而才有这个关键字
AUTO_INCREMENT），给该列赋予下一个可用的值。这样给每个行分配一个
唯一的cust_id，从而可以用作主键值。每个表只允许一个AUTO_INCREMENT列，而且它必须被索引（如，通
过使它成为主键）。

21.1.5 指定默认值

如果在插入行时没有给出值，MySQL允许指定此时使用的默认值。
默认值用CREATE TABLE语句的列定义中的DEFAULT关键字指定。

CREATE TABLE orderitems
(
    order_num int NOT NULL,
    order_item int NOT NULL,
    prod_id char(10) NOT NULL,
    quantity int NOT NULL DEFAULT 1,
    item_price decimal(8,2) NOT NULL,
    PRIMARY KEY (order_num,order_item)
) ENGINE=InnoDB;

(与大多数DBMS不一样，MySQL不允许使用函
数作为默认值，它只支持常量。)

21.1.6 引擎类型

如果省略ENGINE=语句，则
使用默认引擎（很可能是MyISAM），多数SQL语句都会默认使用它。

以下是几个需要知道的引擎：
 InnoDB是一个可靠的事务处理引擎（参见第26章），它不支持全文
本搜索；
 MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘）
中，速度很快（特别适合于临时表）；
 MyISAM是一个性能极高的引擎，它支持全文本搜索（参见第18章），
但不支持事务处理。

21.2 更新表

为了使用ALTER TABLE更改表结构，必须给出下面的信息：
 在ALTER TABLE之后给出要更改的表名（该表必须存在，否则将
出错）；
 所做更改的列表。

ALTER TABLE vendors
ADD vend_phone CHAR(20);

这条语句给vendors表增加一个名为vend_phone的列，必须明
确其数据类型。

21.3 删除表

删除表（删除整个表而不是其内容）非常简单，使用DROP TABLE语
句即可：

DROP TABLE customers2;

这条语句删除customers 2表（假设它存在）。删除表没有确认，
也不能撤销，执行这条语句将永久删除该表。

21.4 重命名表

RENAME TABLE customers2 TO customers;

第 22 章 使 用 视 图

22.1 视图

视图为虚拟的表。它们包含的不是数据而是根据需要检索数据的查询。视图提供了一种MySQL的SELECT语句层次的封装，可用来简化数据处理以及重新格式化基础数据或保护基础数据。

22.2 使用视图

 视图用CREATE VIEW语句来创建。
 使用SHOW CREATE VIEW viewname；来查看创建视图的语句。
 用DROP删除视图，其语法为DROP VIEW viewname;。
 更新视图时，可以先用DROP再用CREATE，也可以直接用CREATE OR
REPLACE VIEW。如果要更新的视图不存在，则第2条更新语句会创
建一个视图；如果要更新的视图存在，则第2条更新语句会替换原
有视图。

22.2.1 利用视图简化复杂的联结

视图的最常见的应用之一是隐藏复杂的SQL

CREATE VIEW productcustomers AS SELECT cust_name,cust_contact,prod_id
FROM customers,orders,orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num;

这条语句创建一个名为productcustomers的视图，它联结三个
表，以返回已订购了任意产品的所有客户的列表。如果执行
SELECT * FROM productcustomers，将列出订购了任意产品的客户。

为检索订购了产品TNT2的客户，

SELECT cust_name,cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';

22.2.2 用视图重新格式化检索出的数据

CREATE VIEW vendorlocations AS
SELECT Concat(RTrim(cend_name),'(',RTrim(vend_country),')') AS vend_title
FROM vendors
ORDER BY vend_name;

SELECT * FROM vendorlocations;

22.2.3 用视图过滤不想要的数据

CREATE VIEW customeremaillist AS 
SELECT cust_id,cust_name,cust_email
FROM customers
WHERE cust_email IS NOT NULL;

SELECT *
FROM customeremaillist;

22.2.4 使用视图与计算字段

视图对于简化计算字段的使用特别有用。

CREATE VIEW orderitemsexpanded AS 
SELECT order_num,
       prod_id,
       quantity,
       item_price,
       quantity*item_price AS expanded_price
FROM orderitems;

SELECT *
FROM orderitemsexpanded
WHERE order_num = 20005;

22.2.5 更新视图

通常，视图是可更新的（即，可以对它们使用INSERT、UPDATE和
DELETE）。更新一个视图将更新其基表（可以回忆一下，视图本身没有数
据）。如果你对视图增加或删除行，实际上是对其基表增加或删除行。

但是，并非所有视图都是可更新的。基本上可以说，如果MySQL不
能正确地确定被更新的基数据，则不允许更新（包括插入和删除）。这实
际上意味着，如果视图定义中有以下操作，则不能进行视图的更新：
 分组（使用GROUP BY和HAVING）；
 联结；
 子查询；
 并；
 聚集函数（Min()、Count()、Sum()等）；
 DISTINCT；
 导出（计算）列

第 23 章 使用存储过程

23.1 存储过程

存储过程简单来说，就是为以后的使用而保存
的一条或多条MySQL语句的集合。

23.3 使用存储过程

23.3.1 执行存储过程

MySQL称存储过程的执行为调用，因此MySQL执行存储过程的语句
为CALL。CALL接受存储过程的名字以及需要传递给它的任意参数。

CALL productpricing(@pricelow,
                    @pricehigh,
              @priceaverage);
            
执行名为productpricing的存储过程，它计算并返回产
品的最低、最高和平均价格。

23.3.2 创建存储过程

一个返回产品平均价格的存储过程.

CREATE PROCEDURE productpricing()
BEGIN
    SELECT Avg(prod_price) AS priceaverage
    FROM products;
END;

此存储过程名为
productpricing，用CREATE PROCEDURE productpricing()语
句定义。如果存储过程接受参数，它们将在()中列举出来。此存储过程没
有参数，但后跟的()仍然需要。BEGIN和END语句用来限定存储过程体，过
程体本身仅是一个简单的SELECT语句

CALL productpricing();

CALL productpricing();执行刚创建的存储过程并显示返回
的结果。因为存储过程实际上是一种函数，所以存储过程名后
需要有()符号（即使不传递参数也需要）。

23.3.3 删除存储过程

DROP PROCEDURE productpricing;

23.3.4 使用参数

一般，存储过程并不显示结果，而是把结果返回给你指定的变量。

CREATE PROCEDURE productpricing(
    OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
    SELECT Min(prod_price)
    INTO pl
    FROM products;
    SELECT Max(prod_price)
    INTO ph
    FROM products;
    SELECT Avg(prod_price)
    INTO pa
    FROM products;
END;

关键字OUT指出相应的参数用来从存储过程传出
一个值（返回给调用者）。MySQL支持IN（传递给存储过程）、OUT（从存
储过程传出，如这里所用）和INOUT（对存储过程传入和传出）类型的参
数。存储过程的代码位于BEGIN和END语句内，如前所见，它们是一系列
SELECT语句，用来检索值，然后保存到相应的变量（通过指定INTO关键
字）。

CALL productpricing(@pricelow,
@pricehigh,
@priceaverage);

所以，这条CALL语句给出3个参数。它们是存储过
程将保存结果的3个变量的名字。在调用时，这条语句并不显示任何数据。它返回以后可以显示（或
在其他处理中使用）的变量。

为了显示检索出的产品平均价格:
SELECT @priceaverage;

使用IN和OUT参数的例子

CREATE PROCEDURE ordertotal(
IN onnumber INT,
OUT ototal DECIMAL(8,2)
)
BEGIN
    SELECT Sum(item_price*quantity)
    FROM orderitems
    WHERE order_num = onumber
    INTO ototal;
END;

onumber定义为IN，因为订单号被传入存储过程。ototal定义
为OUT，因为要从存储过程返回合计。SELECT语句使用这两个
参数，WHERE子句使用onumber选择正确的行，INTO使用ototal存储计算
出来的合计。

CALL ordertotal(20005,@total);


23.3.6 检查存储过程

SHOW CREATE PROCEDURE ordertotal;