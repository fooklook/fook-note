## php内核

内容为阅读《php7内核解析》，做的笔记。

### 面向对象

面向对象编程，简称OOP，是一种程序设计思路。面向对象吧对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

面向对象的特点：

- 封装：将类中的成员属性和方法结合成一个独立的单位，确保类以外的部分不能随意存取类的内部数据。
- 继承：一个类可以继承并拥有另外一个类的成员属性和成员方法。
- 多肽：程序能够处理多种类型对象的能力，PHP中可以通过继承和接口两种方式实现多肽。

### 类

一个类可以包含属于自己的常量、变量（称为“属性”）与函数（称为“方法”），在面向对象中的编程中，类是对象的抽象，对象是类的具体实例。

#### 常量

PHP中可以把在类中始终保持不变的值定义为常量，常量的值必须是一个定值。

```php
class my_class{
    const CONST_NAME = const_value;
    public function __construct(){
        self::CONST_NAME;
    }
}
my_class::CONST_NAME;
```

#### 成员属性

类的属性由关键字 public、protected和private开头，属性中的变量可以初始化，但初始化的值必须是固定不变的值，不能是变量，初始化值在编译阶段时就可以得到其值，而不依赖于运行时的信息才能求值。

#### 成员方法

分为静态方法和动态方法。

#### 类的自动加载

类的自动加载实际上就是内核提供的一个钩子函数，实例化类时如果在EG（class_table）中没有找到对应的类，则会调用这个钩子函数，调用完以后再重新查找一次。

PHP中提供了两种方式实现自动加载，__autoload(),spl_autoload_register()。

spl_autoload_register

相比__autoload只能定义一个加载器，spl_autoload_register提供类更加灵活的注册方式，可以支持任意数量的加载器，比如在不同lib库的文件名规则。

在实现上，spl创建类一个队列来保存用户注册的加载器，然后定义类一个spl_autoload_call函数到EG，当找不到类时，内核回调spl_autoload_call，这个函数在依次调用用户注册的加载器。

#### 魔术方法

- __construct()
- __destruct()
- __call()
- __callStatic()
- __get()
- __set()
- __isset()
- __unset()
- __sleep()
- __wakeup()
- __toString()
- __invoke()
- __set_state()
- __clone()
- __debugInfo()