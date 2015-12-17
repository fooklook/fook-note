##php跨域Ajax请求解决方案
跨域请求是浏览器禁止的行为，一般情况下的做法是使用jsonp，本文介绍一个新的方案。

###通过设置Access-Control-Allow-Origin来实现跨域。
在php的代码中，添加

```php
header("Access-Control-Allow-Origin: *");
```
####只允许单个或多个域名访问

```php
header('Access-Control-Allow-Origin:域名地址1');
header('Access-Control-Allow-Origin:域名地址2');
```
