
## 指定查询的索引
|语法|范围|
|---|---|
|/_search|集群上所有的索引|
|/index1/_search|index1|
|/index1,index2/_search|index1和index2|
|index*/_search|以index开头的索引|

## URI Search
在URL中使用查询参数

`curl -X GET "localhost:9200/twitter/_search?q=user:kimchy`

### 常用查询参数
* q: 指定查询语句，使用Query String Syntax
* df: 默认字段。不指定时会对所有字段进行查询
* sort: 排序字段和排序方式
* from, size: 用于分页
* profile: 可以查看查询是如何被执行的

### 常用查询类型
* 指定字段 v.s 泛查询
  * 指定字段查询：`q=title:2012` 或 `q=2012&df=title`
  * 泛查询：`q=2012`
* Term v.s Phrase 
  * `q=title:(Beautiful Mind)` 等效于Beautiful OR Mind
  * `q=title:"Beautiful Mind"` 等效于 Beautiful AND Mind。Prase查询，还要求前后顺序保持一致
  * *注意*：`q=title:Beautiful Mind` 等效于title字段查询Beautiful，所有字段查询Mind
* 分组与引号
  * `title:(Beautiful AND Mind)`
  * `title:"Beautiful Mind"`
* 布尔操作: AND, OR, NOT
* 范围查询
* 通配符查询
* 模糊匹配，近似度匹配

## Request Body Search
使用Elasticsearch提供的，基于JSON的更加完备的Query DSL(Domain Specific Language)
```
curl -X GET "localhost:9200/twitter/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "term" : { "user" : "kimchy" }
    }
}
'
```

* match query若有空格，默认为OR。若有需要，指定`"operator": "AND"`
* match phrase 查询
```
GET /comments/_doc/_search
{
  "query": {
    "match_phase": {
      "comment": {
        "query": "Song Last Chrismas",
        "slot": 1 // 单词间可以有一个间隔
      }
    }
  }
}
```

## 衡量相关性
Information Retrieval
* Precision (查准率) - 尽可能返回较少的无关文档
`True positive / (True positive + False positive)`
* Recall (查全率) - 尽量返回较多的相关文档
`True positive / (True positive + True negative)`

* Ranking - 是否能够按照相关度进行排序？
