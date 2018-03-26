## php面向对象概念

### 面向对象的特性

#### 三大特性

1. 封装：将某种功能的实现写在一起 延伸出来个接口 让外部调用
2. 继承：对某类事物的公共特性 可以写成基类 供具体功能类继承
3. 多肽：对于某类事物 ，可以拥有相同特性的类 ，写一个基本，其子类可以各自实现自己的需求。

封装是类的构建过程，php具有；php也具有继承的特性。唯独这个多肽，php体现的十分模糊。原因是php是弱类型语言。php不具有像java那种清晰的多肽，不是代表php不具有多肽性。php可以通过以下方式实现多肽。

```php
abstract class animal{
    abstract function fun();
}
class cat extends animal{
    function fun(){
        echo "cat say miaomiao...";
    }
}
class dog extends animal{
    function fun(){
        echo "dog say wangwang...";
    }
}
function work($obj){
    if($obj instanceof animal){
        $obj -> fun();
    }else{
        echo "no function";
    }
}
work(new dog()); 
work(new cat());
```

#### 构造和析构函数

1. 构造函数：实例化类自动执行的函数。
2. 析构函数：当程序结束或者销毁时调用。

#### static

1. 类只有在实例化后才会分配内存 供调用
2. 带有static的属性，方法，类自动分配内存使用时无需实例化等。

#### 辅助函数

1. 判断对象所属的类：
    1. instandof
    2. is_a(最好别用)
    3. is_subclass_of（最好别用）
2. method_exists 判断对象是否存在某方法
3. get_class 获取类名
4. get_parent_class 获取父类名
5. 获取类的属性和方法
    1. 属性：get_class_vars
    2. 方法：get_class_method
6. get_object_vars 获取对象的属性
7. get_declared_class 获取已经申明的类名