## php框架函数

### error_reporting

- 1 E_ERROR 致命的运行错误。错误无法恢复，暂停执行脚本。
- 2 E_WARNING 运行时警告(非致命性错误)。非致命的运行错误，脚本执行不会停止。
- 4 E_PARSE 编译时解析错误。解析错误只由分析器产生。
- 8 E_NOTICE 运行时提醒(这些经常是你代码中的bug引起的，也可能是有意的行为造成的。)
- 16 E_CORE_ERROR PHP启动时初始化过程中的致命错误。
- 32 E_CORE_WARNING PHP启动时初始化过程中的警告(非致命性错)。
- 64 E_COMPILE_ERROR 编译时致命性错。这就像由Zend脚本引擎生成了一个E_ERROR。
- 128 E_COMPILE_WARNING 编译时警告(非致命性错)。这就像由Zend脚本引擎生成了一个E_WARNING警告。
- 256 E_USER_ERROR 用户自定义的错误消息。这就像由使用PHP函数trigger_error（程序员设置E_ERROR）
- 512 E_USER_WARNING 用户自定义的警告消息。这就像由使用PHP函数trigger_error（程序员设定的一个E_WARNING警告）
- 1024 E_USER_NOTICE 用户自定义的提醒消息。这就像一个由使用PHP函数trigger_error（程序员一个E_NOTICE集）
- 2048 E_STRICT 编码标准化警告。允许PHP建议如何修改代码以确保最佳的互操作性向前兼容性。
- 4096 E_RECOVERABLE_ERROR 开捕致命错误。这就像一个E_ERROR，但可以通过用户定义的处理捕获（又见set_error_handler（））
- 8191 E_ALL 所有的错误和警告(不包括 E_STRICT) (E_STRICT will be part of E_ALL as of PHP 6.0)

```php
//禁用错误报告
error_reporting(0);
//报告运行时错误
error_reporting(E_ERROR | E_WARNING | E_PARSE);
//报告所有错误
error_reporting(E_ALL);
//也可以通过设置php.ini文件中，display_errors=On;
```

### compact 多个变量转数组

```php
$name='jb51';
$email='jb51@jb51.net';
$info=compact('name','email');//传递变量名
print_r($info);
  /*
  Array
  (
    [name] => jb51
    [email] => jb51@jb51.net
  )
  */
```

### extract 数组转多个变量

```php
//数组转多个变量
$capitalcities['England'] = 'London';
$capitalcities['Scotland'] = 'Edinburgh';
$capitalcities['Wales'] = 'Cardiff';
extract($capitalcities);//转变成三个变量 England，Scotland，Wales
print $Wales;//Cardiff
```

