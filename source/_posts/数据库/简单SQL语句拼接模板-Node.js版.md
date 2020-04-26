---
title: 简单SQL语句拼接模板-Node.js版
date: 2020-04-26 17:25:37
categories:
- 数据库
comments: false
---

毕业项目的接口编写中，需要写很多查询、修改等 SQL 语句，下面记录简单的 SQL 语句拼接模板，扩展性强，能够满足绝大多数的简单语句拼接
<!-- more -->



### 模板代码

```typescript
// 返回组装好的sql语句
const getSql = {
    // 构造通用的数据库语句结构体
    getSQLObject: function () {
        return {
            // select/update/delete/insert
            "query": "select",
            // table name
            "tables": "",
            "data": {
                // for select, use ("key": anything)
                // for others, use ("key": value)
            },
            "where": {
                // and / or / not / ""
                "type": "and",
                "condition": []
            },
            // options
            "options": {
                "group by": "",
                "order by": ""
            }
        };
    },
    // 构造一个保存 SQL 语句的结构体
    getSQLObject_sv: function () {
        return {
            "sql": "",
            "value": []
        };
    },
    // SQL 语句组装模块，解析数据库结构体
    _structureAnalysis: function (sqlObj) {
        let dataKey = [], dataValue = [];
        let optionKey = [];
        let whereSql = "";
        let hasWhere = false;
        // 读取键值
        for (var key in sqlObj["data"]) {
            if (sqlObj["query"] == 'update') {
                dataKey.push([key, "?"].join("="));
            }
            else {
                dataKey.push(key);
            }
            dataValue.push(sqlObj["data"][key]);
        }

        // 判断是否有where条件
        hasWhere = sqlObj["where"]["condition"].length == 0 ? false : true;
        whereSql = "where " + sqlObj["where"]["condition"].join(` ${sqlObj["where"]["type"]} `);

        // 读取语句中的可选部分(order by, group by)
        for (var key in sqlObj["options"]) {
            if (sqlObj["options"][key] && sqlObj["options"][key] != "") {
                optionKey.push([key, sqlObj["options"][key]].join(" "));
            }
        }

        // 组装语句
        let sql = {
            "update": `update ${sqlObj["tables"]} set ${dataKey.join(",")} ${hasWhere ? whereSql : ""};`,
            "select": `select ${dataKey.join(",")} from ${sqlObj["tables"]} ${hasWhere ? whereSql : ""} ${optionKey.join(" ")};`,
            "delete": `delete from ${sqlObj["tables"]} ${hasWhere ? whereSql : ""};`,
            "insert": `insert into ${sqlObj["tables"]} (${dataKey.join(",")}) values(${dataKey.fill('?').join(",")});`
        }

        // 生成SQL结构体
        let result = getSql.getSQLObject_sv();
        result["sql"] = sql[sqlObj["query"]];
        result["value"] = sqlObj["query"] == "select" ? [] : dataValue;
        return result;
    }
}


module.exports = getSql
export { };
```



### 应用

#### select 语句

```typescript
const findUser = (val) => { // 查找所有User
  let stru = getSQLObject();
  stru["query"] = "select";
  stru["tables"] = "users";
  stru["data"] = {
    "*": '*'
  };
  stru["where"]["condition"] = [
    "name = '" + val + "'",
  ];
  stru["options"]["group by"] = "id";
  let result = _structureAnalysis(stru);
  return result;
}
```



#### insert 语句

```typescript
const insetUser = (val) => { // 注册
  let stru = getSQLObject();
  stru["query"] = "insert";
  stru["tables"] = "users";
  stru["data"] = {
    "name": val.name,
    "password": val.password
  };
  let result = _structureAnalysis(stru);
  return result;
}
```



#### update语句

```typescript
const refuseOrder = (val) => { // 取消订单
  let stru = getSQLObject();
  stru["query"] = "update";
  stru["tables"] = "orders";
  stru["data"] = {
    "refuseReason": val.refuseReason
  };
  stru["where"]["condition"] = [
    "id = " + val.orderId,
  ];
  let result = _structureAnalysis(stru);
  return result;
}
```



#### delete 语句

```typescript
const deleteOrder = (val) => { // 删除订单
  let stru = getSQLObject();
  stru["query"] = "delete";
  stru["tables"] = "orders";
  stru["where"]["condition"] = [
    'id = ' + val.orderId
  ];
  let result = _structureAnalysis(stru);
  return result;
}
```



## 参考文章

[简单SQL语句拼接模块(Nodejs版)](https://blog.csdn.net/think_A_lot/article/details/93500799)

