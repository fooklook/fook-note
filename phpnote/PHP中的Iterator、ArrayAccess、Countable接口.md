##PHP中的Iterator、ArrayAccess、Countable接口

PHP提供的多个接口主要是让对象试用数组的操作方式

###Iterator接口

terator可在内部迭代自己的外部迭代器或类的接口，如使用foreach、while方式来迭代自己

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

```
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

###ArrayAccess接口

ArrayAccess提供像访问数组一样访问对象的能力的接口，就是可以通过中括号索引方式访问元素的能力

```
ArrayAccess {  
        abstract public   offsetExists ( mixed $offset )                //isset($object['name'])时候调用  
        abstract public   offsetGet ( mixed $offset )                   //var_dump($object['name'])时候调用  
        abstract public   offsetSet ( mixed $offset , mixed $value )    //$object['name']='shiki'时候调用  
        abstract public   offsetUnset ( mixed $offset )                 //unset($object['name'])时候调用  
}
```

###Countable接口

让对象可以被用于count函数的能力 

```
Countable {  
        abstract public function count ( )  
}  
```

例子：

```
class  Basket implements Countable{  
    private $fruits =array('apple','banna','pear','orange','watermelon');  
    public function count(){  
        return count($this->fruits);  
    }  
}  
$basket = new Basket();  
var_dump(count($basket)); 
```