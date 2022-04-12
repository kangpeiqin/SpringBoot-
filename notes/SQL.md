<p> <a href="../README.md">返回首页</a></p>

# SQL
## 集合运算
集合在数学领域表示“（各种各样的）事物的总和，**在数据库领域表示记录的集合**。
集合运算，就是对满足同一规则的记录进行的加减等四则运算。
### 并集
- `UNION`：对表进行并集运算，会除去重复的记录
> ORDER BY 子句只能在最后使用一次
- 保留重复行的集合运算：`UNION ALL`
### 交集
使用 INTERSECT 选取出表中公共部分(`MySQL`暂不支持)
### 联结
联结（JOIN ）就是将其他表中的列添加过来，进行“添加列”的集合运算。**UNION 是以行（纵向）为单位进行操作（会导致记录行数的增减），
而联结则是以列（横向）为单位进行的**。
# SQL 练习
- [1873.计算特殊奖金](https://leetcode-cn.com/problems/calculate-special-bonus/)
```SQL
select 
    employee_id,
    case
        when employee_id % 2= 1 and left(name,1) != 'M' 
        then salary
        else 0
    end bonus
from Employees
```
- [175.组合两个表](https://leetcode-cn.com/problems/combine-two-tables/)
```sql
select a.firstName,a.lastName,b.city,b.state from Person a 
left join Address b on a.personId = b.perSonId 
```