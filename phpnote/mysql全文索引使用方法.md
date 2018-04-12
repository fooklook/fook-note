## mysql全文索引使用方法

### 设置全文索引

添加：ALTER TABLE table_name ADD FULLTEXT ( column);

删除：DROP INDEX index_name ON table_name;

注：mysql5.6版本以下只有myisam存储引擎支持全文索引，mysql5.6以上版本myisam和innodb都支持全文索引，两者性能有兴趣了可以比较一下。 

### 搜索语句

SELECT * FROM table_name WHERE MATCH(index_name) AGAINST(‘搜索值’);

多词请用逗号或空格分开：SELECT * FROM table_name WHERE MATCH(index_name) AGAINST(‘a,b’);

到这里基本已经可以使用了，但是有时候在搜索单个字符时候没有结果，这时候需要修改一下全文索引关键词长度设置了。

注：当个别词的出现频率超过50%时，被认作无效词，可以改为AGAINST (‘高频词’ IN BOOLEAN MODE)。

1. 全文索引的字段类型必须为：char,varchar,text 。
2. 对于中文全文索引，必须先把字段值做好中文分词，每个关键词之间用“ ,”“ ”分开，不然即使全文索引还是无效，谁让这些都是老外开发的呢（英文单词之间都是空格，妥妥的），但是中文分词可以借助其他一些开源程序来做，比如：coreseek，附上下载地址：http://www.coreseek.cn/news/7/52/