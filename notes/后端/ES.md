ES 采用 `RESTful` 风格 API
### 获取集群或者节点的信息
```
# 获取集群的健康信息
GET _cluster/health
# 获取节点状态信息
GET _nodes/stats
# 查看所有的索引
GET /_cat/indices?v
```
### CRUD
#### Create
```
# 创建一个索引，用于存储文档集合
PUT index_name
# 添加文档，自动生成主键
POST index_name/_doc
{
  "field": "value"
}
# 添加文档，文档 ID 由用户设置，如果文档存在，则进行覆盖
PUT index_name/_doc/document_id
{
  "field": "value"
}
# 如果文档存在，则报错给出提示信息
PUT index_name/_create/document_id
{
  "field": "value"
}
```
#### Read
```
# 根据文档id读取索引中的文档
GET index_name/_doc/document_id
```
#### Update
```
# 更新指定文档对应的字段和内容
POST index_name/_update/document_id
{
  "doc": {
    "field1": "value",
    "field2": "value",
  }
} 
```
#### Delete
```
# 删除指定文档
DELETE index_name/_doc/id
```
### 搜索信息
```
# 获取索引中的所有文档
GET index_name/_search
# 根据区间查询
GET index_name/_search
{
  "query": {
    "match": {
      "field": {
        "gte": "lowest value",
        "lte": "highest value"
      }
    }
  }
}
# 聚合查询
GET index_name/_search
{
  "aggs": {
    "name your aggregation": {  # 自己定义的分组名称
      "terms": {
        "field": "category", # 需要进行分组的字段
        "size": 100
      }
    }
  }
}
# 全文搜索，会对 search terms 进行分词，根据分词找到匹配的数据
GET index_name/_search
{
  "query": {
    "match": {
      "field": {
        "query": "search terms"
      }
    }
  }
}
# 默认情况下，分词使用 OR 进行文档匹配：如果文档出现了这个分词，就进行返回。可以将操作改成 And：一个文档中同时出现这些分词才回进行返回。
GET index_name/_search
{
  "query": {
    "match": {
      "field you want to search": {
        "query": "search terms",
        "operator": "and"
      }
    }
  }
}
# 短语搜索，将短语当成一个整体进行完全匹配，比如匹配商品的标签
GET index_name/_search
{
  "query": {
    "match_phrase": {
      "field": {
        "query": "search phreas"
      }
    }
  }
}
# 多个字段的内容数据匹配搜索，比如搜索一篇文章标题或者文章主体包含某关键字
GET index_here/_search
{
  "query": {
    "multi_match": {
      "query": "search terms",
      "fields": [
        "field1^2", # 可以设置搜索字段的权重(^)，会排在前面优先展示
        "field2",
        "field you want to search over"
      ],
      "type": "phrase" # 加上这行，表示短语搜索，进行全匹配
    }
  }
}
```
### 组合搜索查询
```
# Bool Query
- must: defines all queries(criteria) a document MUST match to be returned as hits
返回所有条件都符合的搜索结果：必需要符合条件1和条件2...
GET index_name/_search
{
  "query": {
    "bool": {
      "must": [
        {
        "match_phrase": {
          "field1": "value"
         }
        },
        {
          "match": {
            "field2": "value"
          }
        }
      ]
    }
  }
}
- must_not: defines queries(criteria) a document MUST NOT match to be included in the search results
符合某种条件的结果不返回
GET index_name/_search
{
  "query": {
    "bool": {
      "must": {
        "match_phrase": {
          "field1": "value"
         }
        },
       "must_not":[  # 符合这个条件的结果不返回
         {
          "match": {   
            "field2": "value"
          }
        }
      ]
    }
  }
}
- should: "nice to have" queries(criteria)
符合条件的结果会有更高的得分，会展示在搜索结果的更前面
GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
        "match_phrase": {
          "field1": "value"
          }
         }
        ],
       "should":[
         {
          "match_phrase": {
            "field2": "value"
          }
        }
      ]
    }
  }
}
- filter: 过滤符合条件的结果
GET news_headlines/_search
{
  "query": {
    "bool": {
      "must": [
        {
        "match_phrase": {
          "field": "value"
          }
         }
        ],
       "filter":{  # 筛选出符合这个时间区间的结果
          "range":{
             "date": {
               "gte": "begin time",
               "lte": "end time"
          }
        }
      }
    }
  }
}
```

## 参考
- https://github.com/LisaHJung/Beginners-Crash-Course-to-Elastic-Stack-Series-Table-of-Contents
- https://www.elastic.co/guide/cn/elasticsearch/guide/current/_phrase_search.html