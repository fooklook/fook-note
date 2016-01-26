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

- [在windows下安装laravel](./在windows下安装laravel.md)
- [laravel5学堂-开篇](./laravel5学堂-开篇.md)
- [laravel5学堂-路由](./laravel5学堂-路由.md)
- [laravel5学堂-中间件](./laravel5学堂-中间件.md)
- [laravel5学堂-控制器](./laravel5学堂-控制器.md)
- [laravel5学堂-请求及响应](./laravel5学堂-请求及响应.md)
- [laravel5学堂-Artisan](./laravel5学堂-Artisan.md)
- [laravel5学堂-视图](./laravel5学堂-视图.md)
- [laravel5学堂-系统架构](./laravel5学堂-系统架构.md)

###工具使用
程序员学会使用工具，除了能提高开发效率外，关键还很酷。想想，在终端中，优雅的敲上几行命令就能解决问题，就是酷毙了。

- [git-bash工具使用](../toolnote/git-bash工具使用.md)
- [使用git-bash工具实现ssh公钥免密码push代码到github](../toolnote/使用git-bash工具实现ssh公钥免密码push代码到github.md)
- [通过composer发布自己的包](../toolnote/通过composer发布自己的包.md)
- [git笔记](../toolnote/gitnote.md)

###php冷门知识
这些知识在你写代码的时候，可能根本不需要考虑，但是一旦用上了就是救命的，还有这些知识可能对于你的面试有很大的帮助。

你是一位程序员，你去面试一份岗位的时候。请记住一点，你就是为了钱去的。为了抬高身价，理清一些知识是很有必要的。

- [get和post区别](../phpnote/get和post区别.md)
- [php底层运行机制与原理分析](../phpnote/php底层运行机制与原理分析.md)
- [多线层和多进程的区别](../phpnote/多线层和多进程的区别.md)
- [php回调、匿名函数和闭包](../phpnote/php回调、匿名函数和闭包.md)
- [trais](../phpnote/traits.md)
- [IoC容器](../phpnote/IoC容器.md)
- [php密码哈希](../phpnote/php密码哈希.md)
- [ubuntu搭建服务器记录](../phpnote/ubuntu搭建服务器记录.md)
- [ubuntu防火墙设置](../phpnote/ubuntu防火墙设置.md)
- [mysql添加删除用户](../phpnote/mysql添加删除用户.md)
- [http协议](../phpnote/http协议.md)

###php八卦
php被各种吐槽和诟病，所以作为用php开发的phper总在心里面，会觉得比java等一些程序员低一等。

特别是现在ios和android这么火，在工资水平上，这些人就比同等编程经验的phper高一些。

解决问题的第一步，发现问题并且承认问题。

首先我们得承认，总体来说，在市场中的项目，php的代码质量不高。

我总结原因是，首先php入门比较低，不懂编程，或者对编程没有兴趣的人，在培训班或者自学一段时间后，就可以开发项目。

再有，因为现在国内外的php开源框架那么多，很多中层水平的phper并不能和低水平的phper拉开很大的差距。比如公司使用TP框架开发项目，一般情况下，水平较低的程序员，也能胜任项目，或者由一个高水平phper带两个大学刚毕业的phper也能做项目。所以中等水平的phper对学习php就没有了后劲，或者进入一段迷茫期。

市场上对高级phper的需求还是很大的，因为它的开发成本低、开发效率高。再加上，很多人开口闭口就是高并发，但是对于市场上，能有多少公司的并发量是百万级别的。及时这家公司是百万级别的流量，但其中绝大部分的项目也是低流量的。而且高级phper往往等价于全栈工程师，一个人就可以顶起一个甚至是好几个项目。

上面也许写的有点悲观。但并不代表phper没有前途。

希望你们看到以下标题内容，能够兴奋起来。

- [phper快速提现之路](../phpnote/phper快速提现之路.md)
- [Code Review](../phpnote/Code Review.md)
- [面试工作](../phpnote/面试工作.md)