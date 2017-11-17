## mysql的groupby分析

### 说明

在实现分页效果时， 如果sql语句中包含group by时，就比较尴尬了，统计结果条数时，就出错。

本人总结出以下三种处理方式。

### 子语句查询

```mysql
select
count(*) as total
from (select count(*) from users group by user_id) u 
```

### DISTINCT查询

mysql提供 有distinct这个关键字来过滤掉多余的重复记录只保留一条

```mysql
SELECT COUNT(DISTINCT user_id) FROM my_table  
```

虽然mysql提供 有distinct这个关键字来过滤掉多余的重复记录只保留一条，但往往只用它来返回不重复记录的条数，而不是用它来返回不重记录的所有值。其原因是 distinct只能返回它的目标字段，而无法返回其它字段。

### FOUND_ROWS查询

用FOUND_ROWS()就能获得查询总数，这个数目是抛掉了LIMIT之后的结果数。

关键函数：SQL_CALC_FOUND_ROWS、FOUND_ROWS

```mysql
SELECT SQL_CALC_FOUND_ROWS * FROM `table` WHERE ...... limit M, N;
SELECT FOUND_ROWS();
```