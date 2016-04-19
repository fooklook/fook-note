##php设计模式

###单例模式

单例模式顾名思义，就是只有一个实例。作为对象的创建模式， 单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

####单例模式的要点有三个：

1. 一是某个类只能有一个实例；
2. 二是它必须自行创建这个实例；
3. 三是它必须自行向整个系统提供这个实例。

####为什么要使用PHP单例模式

1. php的应用主要在于数据库应用, 一个应用中会存在大量的数据库操作, 在使用面向对象的方式开发时, 如果使用单例模式, 则可以避免大量的new 操作消耗的资源,还可以减少数据库连接这样就不容易出现 too many connections情况。
2. 如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现. 这个可以参看zend Framework的FrontController部分。
3. 在一次页面请求中, 便于进行调试, 因为所有的代码(例如数据库操作类db)都集中在一个类中, 我们可以在类中设置钩子, 输出日志，从而避免到处var_dump, echo。

```php
/**
 * 设计模式之单例模式
 * $_instance必须声明为静态的私有变量
 * 构造函数必须声明为私有,防止外部程序new类从而失去单例模式的意义
 * getInstance()方法必须设置为公有的,必须调用此方法以返回实例的一个引用
 * ::操作符只能访问静态变量和静态函数
 * new对象都会消耗内存
 * 使用场景:最常用的地方是数据库连接。
 * 使用单例模式生成一个对象后，该对象可以被其它众多对象所使用。
 */
class man
{
    //保存例实例在此属性中
    private static $_instance;

    //构造函数声明为private,防止直接创建对象
    private function __construct()
    {
        echo '我被实例化了！';
    }

    //单例方法
    public static function get_instance()
    {
        var_dump(isset(self::$_instance));
        
        if(!isset(self::$_instance))
        {
            self::$_instance=new self();
        }
        return self::$_instance;
    }

    //阻止用户复制对象实例
    private function __clone()
    {
        trigger_error('Clone is not allow' ,E_USER_ERROR);
    }

    function test()
    {
        echo("test");

    }
}

// 这个写法会出错，因为构造方法被声明为private
//$test = new man;

// 下面将得到Example类的单例对象
$test = man::get_instance();
$test = man::get_instance();
$test->test();

// 复制对象将导致一个E_USER_ERROR.
//$test_clone = clone $test;
```

###简单工厂模式

1. 抽象基类：类中定义抽象一些方法，用以在子类中实现
2. 继承自抽象基类的子类：实现基类中的抽象方法
3. 工厂类：用以实例化所有相对应的子类

http://www.cnblogs.com/siqi/archive/2012/09/09/2667562.html