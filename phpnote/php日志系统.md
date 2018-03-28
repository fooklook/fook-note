## php日志系统

### php日志相关函数

```php
set_error_handler(array("Myexception","errorHandler"));                 //错误捕获自定义处理函数注册
set_exception_handler(array("Myexception","exceptionHandler"));         //异常捕获自定义处理函数注册
register_shutdown_function(array("Myexception","shutdownHandler"));     //程序执行时异常终止错误捕获处理函数注册
class Myexception{
	public static function exceptionHandler(Exception $e){
		echo 'error exception'."\n\r";
		var_dump($e);
	}
	public static function errorHandler($Code, $Message, $File, $Line,$Context){
		echo 'error handler'."\n\r";
		var_dump($Message);
	}
	public static function shutdownHandler(){
		var_dump('system is shutdown');
	}
}
var_dump($a);									//这行将会出发error
throw new Exception("Catch me twice", 1);		//这行会出发exception
```