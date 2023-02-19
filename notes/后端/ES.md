### 获取集群或者节点的信息
```
# 获取集群的健康信息
GET _cluster/health
# 获取节点状态信息
GET _nodes/stats
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
# 如果文档存在，则给出提示
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
```
















## 参考
- https://github.com/LisaHJung/Part-2-Understanding-the-relevance-of-your-search-with-Elasticsearch-and-Kibana-