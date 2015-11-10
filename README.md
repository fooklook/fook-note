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