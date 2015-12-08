##php回调、匿名函数和闭包

首先了解几个概念

- 匿名函数就是闭包，哈哈。
- 类中的静态方法，也是回调函数。
- 匿名函数和回调函数的区别，就是回调函数将函数赋给了变量，而匿名函数没有。呵呵。
- 回调有什么用？利用回调函数，你可以将与组件核心任务没有直接关系的功能插入到组件中，有了组件回调，你就赋予了其他人在不知道的你代码的情况下，扩展你代码权利。匿名函数也是类似的。

```php
//回调函数的一些特性
class class_name{
    private $callback;
    public function function_name(callback $callback){
        if(is_callback($callback)){
            $this->callback[] = $callback;
        }
    } 
}
//匿名函数的一些特性
class class_name{
    private $name;
    public function function_name($function){
        if($function instancdof colsure){
            $this->name[] = $function;
        }
    }
}
$num = 10;
$class = new class_name();
$class->function_name(function() use $num {
  return $num;
});
```