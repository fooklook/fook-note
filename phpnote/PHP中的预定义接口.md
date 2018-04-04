## PHP的预定义接口

PHP提供的多个接口主要是让对象试用数组的操作方式

### Traversable（遍历）接口

Traversable接口实际上不是一个接口，在实际写php代码中不能用。因为只有内部的PHP类（用C写的类）才可以直接实现Traversable接口。可以说这是个特性级别的东西。

实际的PHP的Iterator接口IteratorAggregate接口就是继承了这个接口实现的。

特性：

1. Traversable 接口不能直接实现（implements）.
2. Traversable 重要的一个用处就是判断一个类是否可以遍历：

```php
if($class instanceof Traversable)
{
     //foreach...
}
```

### Iterator（迭代器）接口

Iterator接口的主要用途是允许一个类实现一个基本的迭代功能，从而使它可以被循环访问，根据键值访问以及回滚。Iterator接口摘要如下：

接口的定义如下: 

```
Iterator extends Traversable {
    abstract public function current ()  
    abstract public function key ()  
    abstract public function next ()  
    abstract public function rewind ()  
    abstract public function valid ()  
}  
```

举例：

```php
class  Basket implements Iterator{  
    private $fruits =array('apple','banna','pear','orange','watermelon');  
    //现在的位置  
    private $posistion =0;  
    /** 
    * 遍历到现在的值是什么 
    */  
    public  function current (){  
        return $this->fruits[$this->posistion];  
    }  
    /** 
    * 遍历到现在的key是什么 
    */  
    public  function key (){  
        return $this->posistion;  
    }  
    /** 
    * 遍历下一个 
    */  
    public  function next (){  
        ++$this->posistion;  
    }  
    /** 
    * foreach变量开始时自动调用 
    */  
    public  function rewind (){  
        $this->posistion =0;  
    }  
    /** 
    * 判断现在的key是否是合理，返回true则遍历，false则停止遍历 
    */  
    public  function valid (){  
        if($this->posistion<count($this->fruits)) return true;  
        return false;  
    }  
}  


$basket  = new Basket();  
foreach($basket as $key=>$fruit){  
    echo $key ,'--',$fruit,"\n";  
}  
//也可用下面的遍历方式，效果一样  
while($basket->valid()){  
    echo $basket->key(),'--',$basket->current(),"\n";  
    $basket->next();  
}  
```

### IteratorAggregate（聚合式迭代器）接口

IteratorAggregate又叫聚合式迭代器。创建外部迭代器的接口，其摘要如下：

```php
IteratorAggregate extends Traversable { 
    //实现该方法时，必须返回一个实现了Iterator接口的类的实例 
    abstract public Traversable getIterator ( void ) 
} 
```

其中getIterator方法返回值必须是能遍历或实现Iterator接口（must be traversable or implement interface Iterator）。SPL还提供了一些专门用来与IteratorAggregate接口一起使用的内置迭代器。使用这些迭代器意味着只需要实现一个方法并实例化一个类就可以使对象可以迭代访问了。

```php
class myData implements IteratorAggregate  
{  
    public $property1 = "公共属性1";  
    public $property2 = "公共属性2";  
    public $property3 = "公共属性3";  
  
    public function __construct()  
    {  
        $this->property4 = "最后一个公共属性";  
    }  
  
    public function getIterator()  
    {  
        return new ArrayIterator($this);  
    }  
}
$obj = new myData;  
foreach ($obj as $key => $value) {  
    echo "键名：{$key}  值：{$value}\n";  
}
//程序输出
键名：property1  值：公共属性1
键名：property2  值：公共属性2
键名：property3  值：公共属性3
键名：property4  值：最后一个公共属性
```

### ArrayAccess （数组式访问）接口

ArrayAccess提供像访问数组一样访问对象的能力的接口，就是可以通过中括号索引方式访问元素的能力

```php
class ArrayAccess {  
        abstract public   offsetExists ( mixed $offset )                //isset($object['name'])时候调用  
        abstract public   offsetGet ( mixed $offset )                   //var_dump($object['name'])时候调用  
        abstract public   offsetSet ( mixed $offset , mixed $value )    //$object['name']='shiki'时候调用  
        abstract public   offsetUnset ( mixed $offset )                 //unset($object['name'])时候调用  
}
```

```php
class obj implements Arrayaccess {
    private $container = array();
    public function __construct() {
        $this->container = array(
            "one"   => 1,
            "two"   => 2,
            "three" => 3,
        );
    }
    public function offsetSet($offset, $value) {
        if (is_null($offset)) {
            $this->container[] = $value;
        } else {
            $this->container[$offset] = $value;
        }
    }
    public function offsetExists($offset) {
        return isset($this->container[$offset]);
    }
    public function offsetUnset($offset) {
        unset($this->container[$offset]);
    }
    public function offsetGet($offset) {
        return isset($this->container[$offset]) ? $this->container[$offset] : null;
    }
}
$obj = new obj;
var_dump(isset($obj["two"]));
var_dump($obj["two"]);
unset($obj["two"]);
var_dump(isset($obj["two"]));
$obj["two"] = "A value";
var_dump($obj["two"]);
$obj[] = 'Append 1';
$obj[] = 'Append 2';
$obj[] = 'Append 3';
print_r($obj);
```

### Serializable

序列化接口。实现该接口的类不能使用__sleep()和__wakeup().在serialize时不执行__destruct()，在unserialize不执行__construct()。

```php
Serializable {
	/* Methods */
	abstract public string serialize ( void )                   //对象的字符串表示
	abstract public mixed unserialize ( string $serialized )    //构造对象
}
```

```php
class obj implements Serializable {
    private $data;
    public function __construct() {
        $this->data = "My private data";
    }
    public function serialize() {
        return serialize($this->data);
    }
    public function unserialize($data) {
        $this->data = unserialize($data);
    }
    public function getData() {
        return $this->data;
    }
}
$obj = new obj;                 //调用obj::__construct
$ser = serialize($obj);         //调用obj::serialize
$newobj = unserialize($ser);    //调用obj::unserialize
var_dump($newobj->getData());   //调用obj::getData
```

### Closure 类

用于代表匿名函数的类。

```php
//闭包概念
$a = function($args){  
    echo "i haven't name";  
    echo "=".$args;  
};  
$a(11); 
//bind用法
class user{  
    private $money = 200;  
    public $level = 0;  
    public function beatMonster(){  
        echo "beat monster!";  
    }  
}  
//进入游戏之后新建一个角色xf  
$xf = new user;  
$uplevel = function(){  
    $this->level++;  
};
$upmylevel = closure::bind($uplevel,$xf,'user');  //第一个参数为bind的匿名函数，第二个参数为bind的对象，第三个参数为bind的作用域。
//bindTo用法
class user{  
    private $money = 200;  
    public $level = 0;  
    public function beatMonster(){  
        echo "beat monster!";  
    }  
}  
//进入游戏之后新建一个角色xf  
$xf = new user;  
$uplevel = function(){  
    $this->level++;  
};  
$upmylevel = $uplevel->bindTo($xf,'user');  
```

### Countable

让对象可以被用于count函数的能力 

```php
class Countable {  
        abstract public function count ( )  
}  
```

例子：

```php
class  Basket implements Countable{  
    private $fruits =array('apple','banna','pear','orange','watermelon');  
    public function count(){  
        return count($this->fruits);  
    }  
}  
$basket = new Basket();  
var_dump(count($basket)); 
```

### 生成器类

Generator类，不能被实例化。

关键字yield

函数yield返回的是Generator类。

