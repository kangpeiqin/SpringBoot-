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
GET Enter_name_of_index_here/_search
{
  "query": {
    "match": {
      "field": {
        "query": "search terms"
      }
    }
  }
}
# 短语搜索，将短语当成一个整体进行搜索

```
















## 参考
- https://github.com/LisaHJung/Beginners-Crash-Course-to-Elastic-Stack-Series-Table-of-Contents
- https://www.elastic.co/guide/cn/elasticsearch/guide/current/_phrase_search.html