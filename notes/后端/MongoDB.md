## 聚合查询
```
//聚合分页查询，并获取总数
db.collection.aggregate([
    {
        $group: {
            _id: { $dateToString: {format: "%Y-%m-%d",date: '$createTime'} },
            num: {
                $sum: 1
            }
        }
    },
    {
        "$facet":{
            total: [{ $count: 'count'}],
            data: [{$sort:{"_id":-1}},{$skip:0},{$limit:1}]
        }
    }
])
//对应的代码查询示例
mongoTemplate.aggregate(Aggregation.newAggregation(
        Aggregation.match(timeCriteria), Aggregation.project("createTime").and("createTime").dateAsFormattedString("%Y-%m-%d").as("date"),
        Aggregation.group("date").count().as("count"), Aggregation.project("_id", "count").and("_id").as("date"),
        Aggregation.facet(Aggregation.count().as("count")).as("total").and(Aggregation.sort(sort, "date"), 
        Aggregation.skip(Long.valueOf(startRows)), Aggregation.limit(pageSize)).as("data")
), xxx.class,xxx.class).getUniqueMappedResult();
```