##xunsearchnote
###Xunsearch(讯搜)搜索引擎的优势：

- 提供大量内置可商业化搜索功能。
- 接口简单易用，官方文档全面。
- 支持scws中文分词、拼音搜索（这两点是其它搜索引擎不具备的）。
- 核心搜索功能基于C/C++开发稳定高效，结合php使得开发效率大大提高。
- 健壮的后端守护程序，内置缓存设计，事件模型基于 libevent。

Xunsearch(讯搜)搜索引擎的劣势：

- 适合千、百万级别数据的索引。和其它索引相比效率较低。
- 文档较少，且没有活跃的社区。

Xunsearch内置适合商业化使用索引功能：

字段检索、结果高亮、 字段排序、布尔语法、区间检索、聚合搜索、相关搜索、权重微调、拼音搜索、 搜索建议等等专业搜索引擎具备的功能。

个人总结：  
首先xunsearch主要的有点是支持中文分词、拼音检索和中文搜索，这些可能其他的引擎是做不到的，而且足够轻量级，易于安装。  

如果企业网站中需要中文检索系统，而网站内容在千万级别内的，xunsearch是足够满足企业需求的。而且它开发简单，易于上手，是减少企业开发成本的优先选择。

###安装xunsearch
首先xunsearch必须在linux环境下安装。如果你是windows系统，也不用着急。现在的电脑的配置已经很高了，所以安装个虚拟机不是问题。如果是win 10专业版以上的用户，强烈建议使用win 10自带的虚拟机Hyper-V(使用方式自行百度)。实在不想安装，也可以在阿里云上租个服务器。

我的服务器是ubuntu14.04-SERVER的版本（桌面系统是鸡肋）。

首先要确保ubuntu安装了gcc g++ make

```shell
sudo apt-get install make gcc g++
```
然后安装zlib，用来解压的：

```shell
sudo apt-get install zlib1g-dev 
```
下载和安装
```shell
wget http://www.xunsearch.com/download/xunsearch-full-latest.tar.bz2//下载xunsearch安装包
tar -xjf xunsearch-full-latest.tar.bz2//解压在当前目录
cd xunsearch-full-1.3.0//进入解压目录
sh setup.sh//安装
//根据提示输入xunsearch安装目录，建议安装在/usr/local目录下。等待安装...
```
开启xunsearch  
$prefix/bin/xs-ctl.sh start  
绑定全部IP，适合SDK/服务端 不同服务器的情况  
$prefix/bin/xs-ctl.sh -b inet start  
停止服务器，若启动时指定了-b inet此处也必须指定  
$prefix/bin/xs-ctl.sh stop  
###xunsearch开发
####总结
xunsearch提供了丰富的类似数据库CURD的API，开发起来非常方便。在开发时你可能遇到的主要问题是，索引字段的设定，还有就是命名空间的问题。

应用场景：按字段值分面索引。

在搭建商城购物网站时，你可能需要开发一个功能，用户通过勾选品牌等其他产品属性，对搜索结果进行过滤。
这个功能，在文档中提的比较少。
>举例说明品牌的条件过滤(brand)。
>>字段属性的设定  
>>[brand]  
>>type=string  
>>index=self	//允许通过字段检索  
>>tokenizer = full	//字段本身只能被整体检索

在设计数据库时，通常的做法是将品牌名称和商品分表，然后通过id关联。但在xunsearch系统中，不必要这么做，brand这个字段直接存储品牌名称。  
代码：

```php
$xs = new XS();
$search = $xs->getSearch();
$search->setQuery('关键词');
$search->addQueryTerm('brand','品牌名称');//
$search->addQueryTerm('column','其他列');
$search->setFacets(array('brand', 'column'));
// 读取分面结果
$brand_counts = $search->getFacets('brand'); // 返回数组，以 brand 为键，匹配数量为值
$brand_counts = $search->getFacets('column'); // 返回数组，以 column 为键，匹配数量为值
// 遍历 $fid_counts, $year_counts 变量即可得到各自筛选条件下的匹配数量
foreach ($fid_counts as $fid => $count)
{
    echo "其中版块ID为 $fid 的匹配数为: $count\n";
}
```

命名空间，现在很多php框架都是基于命名空间了，所以要将xunsearch的SDK整合到框架中，很多人就打了退堂鼓。我只想明确的告诉大家，xunsearch的SDK整合到命名空间中不麻烦。需要修改的地方不多。