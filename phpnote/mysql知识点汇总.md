## mysql知识点汇总

### 索引

### 数据库缓存

### myIsam底层原理及知识点

### InnoDB

### MyIsam与InnoDB

### 小知识点

#### 前缀索引

如果需要的索引是很长的字符列，这会让索引变的大且慢。且字符串的前缀差异较大，就可以使用前缀索引。

```
# 语法
ALTER TABLE table_name ADD KEY(column_name(prefix_length));
# 示例
ALTER TABLE city ADD KEY(cityname(7))
```

#### 单列缩影

#### 多列缩影

#### 复合索引又名联合索引

#### 聚集索引和非聚集索引




