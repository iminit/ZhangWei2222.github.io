---
title: Elastic Search基础
date: 2019-12-06 09:47:37
categories:
- 后端知识
tags:
comments: false
---

## 安装 

### Mac 安装教程
https://blog.csdn.net/Sibylsf/article/details/90636509

### 杀掉ES进程

1. 查找ES进程

  ```js
  ps -ef | grep elastic
  ```

2. 杀掉ES进程
  
   ```js
   kill -9 2382（进程号）
   ```
   
3. 重启ES
  
   ```js
   sh elasticsearch -d
   ```
   

### kibana
https://www.cnblogs.com/yangxiaoyi/p/7234222.html

## 简介

Elasticsearch是一个实时分布式搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。它用于全文搜索、结构化搜索、分析以及将这三者混合使用：

* 维基百科使用Elasticsearch提供全文搜索并高亮关键字，以及输入实时搜索(search-as-you-type)和搜索纠错(did-you-mean)等搜索建议功能。 

* 英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈，以便及时了解公众对新发表的文章的回应。 

* StackOverflow结合全文搜索与地理位置查询，以及more-like-this功能来找到相关的问题和答案。 

* Github使用Elasticsearch检索1300亿行的代码。 

  

### 集群和节点
- 节点(node)是一个运行着的Elasticsearch实例。
- 集群(cluster)是一组具有相同cluster.name的节点集合，他们协同工作，共享数据并提供故障转移和扩展功能，当然一个节点也可以组成一个集群。你最好找一个合适的名字来替代cluster.name的默认值，比如你自己的名字，这样可以防止一个新启动的节点加入到相同网络中的另一个同名的集群中。
- 索引(indexing)：存储数据的行为
- 索引(index)：在Elasticsearch中，文档归属于一种类型(type),而这些类型存在于索引(index)

> Relational DB -> Databases -> Tables -> Rows -> Columns
> Elasticsearch -> Indices   -> Types  -> Documents -> Fields
> Elasticsearch集群可以包含多个索引(indices)（数据库），每一个索引可以包含多个类型(types)（表），每一个类型包含多个文档(documents)（行），然后每个文档包含多个字段(Fields)（列）。

* 索引（名词） 如上文所述，一个索引(index)就像是传统关系数据库中的数据库，它是相关文档存储的地方，index的复数是indices 或indexes。
* 索引（动词） 「索引一个文档」表示把一个文档存储到索引（名词）里，以便它可以被检索或者查询。这很像SQL中的INSERT关键字，差别是，如果文档已经存在，新的文档将覆盖旧的文档。
* 倒排索引 传统数据库为特定列增加一个索引，例如B-Tree索引来加速检索。Elasticsearch和Lucene使用一种叫做倒排索引(inverted index)的数据结构来达到相同目的。默认情况下，文档中的所有字段都会被索引（拥有一个倒排索引），只有这样他们才是可被搜索的。




### 索引

```js
PUT /megacorp/employee/2
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}

GET /megacorp/employee/2 
// 可以看到内容在source中
{
  "_index" : "megacorp",
  "_type" : "employee",
  "_id" : "2",
  "_version" : 1,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "first_name" : "Jane",
    "last_name" : "Smith",
    "age" : 32,
    "about" : "I like to collect rock albums",
    "interests" : [
      "music"
    ]
  }
}

DELETE /megacorp/_doc/2 删除
```




## 搜索

### 简单搜索
GET /megacorp/employee/_search 检查表中所有文档，默认情况下搜索会返回前10个结果

GET /megacorp/employee/_search?q=last_name:Smith 将查询语句传递给参数q=。这样就可以得到所有姓氏为Smith的结果

### 使用DSL语句查询
为了避免通过命令行搜索的局限性(简单搜索  返回全部)，我们可以使用DSL语句查询：DSL(Domain Specific Language特定领域语言)以JSON请求体的形式出现。

```js
GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "last_name" : "Smith"
        }
    }
}
```

使用match语句

使用match语句


### 更复杂的搜索

添加过滤器(filter),

```js
GET /megacorp/employee/_search
{
    "query" : {
        "bool" : {
            "filter" : {
                "range" : {
                    "age" : { "gt" : 30 } <1> 
                }
            },
            "must" : {
                "match" : {
                    "last_name" : "smith" <2> 
                }
            }
        }
    }
}
```

* <1> 这部分查询属于区间过滤器(range filter),它用于查找所有年龄大于30岁的数据——gt为"greater than"的缩写。
* <2> 这部分查询与之前的match语句(query)一致。

### 全文搜索

```js
GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
```

会根据相关性对结果集进行排序，以上案例返回第一个”"I love to go rock climbing" 和 第二个”"I like to collect rock albums"


### 短语搜索

确切的匹配若干个单词或者短语(phrases)。例如我们想要查询同时包含"rock"和"climbing"（并且是相邻的）的员工记录。
只要将match查询变更为`match_phrase`查询即可:

```js
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
```

和上面的全文搜索相比，就没有"I like to collect rock albums”


### 高亮搜索
```js
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
```

当我们运行这个语句时，会命中与之前相同的结果，但是在返回结果中会有一个新的部分叫做highlight，这里包含了来自about字段中的文本，并且用<em></em>来标识匹配到的单词




## 聚合

类似于sql的`GROUP BY`

```js
GET /megacorp/employee/_search
{
 "aggs": {
  "all_interests": {
   "terms": { "field": "interests.keyword" }
  }
 }
}
```

会得出匹配总和数

```js
{
   ...
   "hits": { ... },
   "aggregations": {
      "all_interests": {
         "buckets": [
            {
               "key":       "music",
               "doc_count": 2
            },
            {
               "key":       "forestry",
               "doc_count": 1
            },
            {
               "key":       "sports",
               "doc_count": 1
            }
         ]
      }
   }
}
```

### 缩小范围

如果我们想缩小范围，只需要再加一层query 搜索一下即可

```js
GET /megacorp/employee/_search
{
  "query": {
    "match": {
      "last_name": "smith"
    }
  },
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests.keyword"
      }
    }
  }
}
```

### 聚合中分级汇总

```js
GET /megacorp/employee/_search
{
    "aggs" : {
        "all_interests" : {
            "terms" : { "field" : "interests.keyword" },
            "aggs" : {
                "avg_age" : {
                    "avg" : { "field" : "age" }
                }
            }
        }
    }
}
...
"all_interests": {
  "buckets": [
    {
      "key": "music",
      "doc_count": 2,
      "avg_age": {
        "value": 28.5
      }
    },
    {
      "key": "forestry",
      "doc_count": 1,
      "avg_age": {
        "value": 35
      }
    },
    {
      "key": "sports",
      "doc_count": 1,
      "avg_age": {
        "value": 25
      }
    }
  ]
}
```



## 文档

### 文档元数据

一个文档不只有数据。它还包含了**元数据(metadata)**——**关于**文档的信息。三个必须的元数据节点是：

| 节点     | 说明               |
| -------- | ------------------ |
| `_index` | 文档存储的地方     |
| `_type`  | 文档代表的对象的类 |
| `_id`    | 文档的唯一标识     |



## 索引
### 使用自己的ID

```js
PUT /{index}/{type}/{id}
{
  "field": "value",
  ...
}
```

### 自增ID
注意使用的是`POST`
```js
POST /website/blog/
{
  "title": "My second blog entry",
  "text":  "Still trying this out...",
  "date":  "2014/01/01"
}
```



## 获取

```js
// 具体获取某个ID文档，pretty表美化
GET /website/blog/123?pretty

// 返回_source的一部分
GET /website/blog/123?_source=title,text

// 只要_source字段，不要其他元数据
GET /website/blog/123/_source
```



## 删除

如果文档被找到，返回200并且`_version`会增加。没找到要删除的文档，会得到404


```js
DELETE /website/blog/123
```

> 删除一个文档也不会立即从磁盘上移除，它只是被标记成已删除。Elasticsearch将会在你之后添加更多索引的时候才会在后台进行删除内容的清理。