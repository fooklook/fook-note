##学习laravel笔记
内容仅供参考，如果发现有错误或者意见，请发送邮件至xiashuo.he@foxmail.com

- 今天尝试使用netbeans打开laravel项目，发现有很多错误，主要在数组的声明上。laravel喜欢这样声明数组
```php
	$array = [213,456];
```
而比较正规的方式是
```php
	$array = array(213,456);
```
，建议大家使用后者。
###laravel生命周期图
![larave生命周期图](http://7xo7bi.com1.z0.glb.clouddn.com/20151126202335.png?imageView2/2/w/600)

###laravel学习目录
首先这里的笔记，绝不仅仅是摘抄文档或者别人的博客这么简单，里面有一些文档中没有写到，个人对框架这样设计的综合理解。

- [在windows下安装laravel](./%E5%9C%A8windows%E4%B8%8B%E5%AE%89%E8%A3%85laravel.md)
- [laravel5学堂-开篇](./laravel5%E5%AD%A6%E5%A0%82-%E5%BC%80%E7%AF%87.md)
- [laravel5学堂-路由](./laravel5%E5%AD%A6%E5%A0%82-%E8%B7%AF%E7%94%B1.md)
- [laravel5学堂-中间件](./laravel5%E5%AD%A6%E5%A0%82-%E4%B8%AD%E9%97%B4%E4%BB%B6.md)
- [laravel5学堂-控制器](./laravel5%E5%AD%A6%E5%A0%82-%E6%8E%A7%E5%88%B6%E5%99%A8.md)
- [laravel5学堂-请求及响应](./laravel5%E5%AD%A6%E5%A0%82-%E8%AF%B7%E6%B1%82%E5%8F%8A%E5%93%8D%E5%BA%94.md)
- [laravel5学堂-Artisan](./laravel5%E5%AD%A6%E5%A0%82-Artisan.md)
- [laravel5学堂-视图](./laravel5%E5%AD%A6%E5%A0%82-%E8%A7%86%E5%9B%BE.md)

###工具使用
程序员学会使用工具，除了能提高开发效率外，关键还很酷。想想，在终端中，优雅的敲上几行命令就能解决问题，就是酷毙了。

- [git-bash工具使用](./toolnote/git-bash%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8.md)
- [使用git-bash工具实现ssh公钥免密码push代码到github](./toolnote/%E4%BD%BF%E7%94%A8git-bash%E5%B7%A5%E5%85%B7%E5%AE%9E%E7%8E%B0ssh%E5%85%AC%E9%92%A5%E5%85%8D%E5%AF%86%E7%A0%81push%E4%BB%A3%E7%A0%81%E5%88%B0github.md)

###php冷门知识
这些知识在你写代码的时候，可能根本不需要考虑，但是一旦用上了就是救命的，还有这些知识可能对于你的面试有很大的帮助。

你是一位程序员，你去面试一份岗位的时候。请记住一点，你就是为了钱去的。为了抬高身价，理清一些知识是很有必要的。

- [get和post区别](./phpnote/get%E5%92%8Cpost%E5%8C%BA%E5%88%AB.md)
- [php底层运行机制与原理分析](./phpnote/php%E5%BA%95%E5%B1%82%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6%E4%B8%8E%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90.md)
- [多线层和多进程的区别](./phpnote/%E5%A4%9A%E7%BA%BF%E5%B1%82%E5%92%8C%E5%A4%9A%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%8C%BA%E5%88%AB.md)
- [php回调函数](phpnote/php%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0.md)
