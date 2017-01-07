##mysql实用查询

###存在则更新，不存在则插入
常规方法解决该问题的方式是，先通过select语句查询，查询结果为空则进行插入，查询结果不为空，则进行更新。这种方法要写三条语句，执行两次数据库操作，效率比较低。

在mysql语法中存在两种方法用户实现该功能，但是应用场景不同。

```mysql
//replace方法
//column1为唯一索引
REPLACE INTO tablename(`column1`,`column2`) VALUES(value1,value2);
```
因为column1为唯一索引，如果数据库中的column1字段存在value1，则更新column2字段的值为value2。如果不存在，则插入这条数据。

```mysql
//INSERT … ON DUPLICATE KEY UPDATE语法,mysql4.1以上支持使用
//首先将column1设为唯一索引，保证它在字段中是唯一的。
INSERT INTO tablename VALUES(`column1`=value1,clicks=1) ON DUPLICATE KEY UPDATE clicks=clicks+1;
```
因为column1为唯一索引，如果数据库中的column1字段不存在value1，则创建一个新的数据，如果存在，则将clicks这个字段的值加一。

从上面的语句，就可以知道，这两个有不同的用法。replace方法不能进行类似于clicks+1这样的操作。

###group_concat
table1  
|--id  
|--name  

table2  
|--id  
|--table1_id  
|--bookname  

上面为两个表结构，一个人有多本书，现在要查出前面是个人他们的书名。通过一条sql语句实现

```mysql
SELECT id, name, group_concat(bookname) from table1 LFET JOIN table2 ON(table1.id=table2.table1_id) WHERE id<10 group by id;
```

###查询结果顺序按IN关键字中ID的排列

```mysql
SELECT * FROM items WHERE item_id IN( 2, 1, 3, 9, 7) ORDER BY FIELD( item_id, 2, 1 ,3 ,9 , 7 )
```

###用一条sql语句统计某一字段等于不同值的个数

```mysql
select id,sum(case when type=0 then 1 else 0 end) as '0',sum(case when type=1 then 1 else 0 end) as '1' from t group by id
```