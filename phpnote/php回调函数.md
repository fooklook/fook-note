##php回调函数
作为一位号称略有小成的phper,除了一些php的字符串处理回调函数，很少用到这种语法。

在学习laravel后，发现很多“服务提供者”为了解耦和使代码更有封装性，在提供接口时，通常通过回调函数的方式，提供相关的内部逻辑处理。

###基本语法

```php
//普通使用方法
//回调函数
function callback($param, $function){
	return $function($param);
}
//调用回调函数
echo callback('callbackparam',function($param){
	return $param;
});
//这种语法一般人都能看懂，但是它在什么情况下使用，使用的技巧是什么呢？我写个例子：
class book{
	private function __contruct(){};
	private $bookname;
	private $booknum;
	private $instance=null;
	static public function instance(){
		if(is_null(self::$instance)){
			self::$instance = new self();
		}
	}
	public function work($function){
		return $function($this->instance);
	}
	private function showbook(){
		return $this->bookname. ':' .$this->booknum;
	}
}
echo book::instance()->work(function($book){
	$book->showbook();
})
//通过这种做法，就可以减少耦合，将所有的接口封装在类里面。
```