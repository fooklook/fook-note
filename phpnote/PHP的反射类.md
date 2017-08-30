##PHP的反射类

###ReflectionClass

####获取类基本信息

```
$ref = new ReflectionClass($classname);
echo $ref->getName();                   //获取类名
echo $ref->getFileName();               //获取类所在的文件名
```

####获取类属性信息

参数：

- ReflectionProperty::IS_STATIC
- ReflectionProperty::IS_PUBLIC
- ReflectionProperty::IS_PROTECTED
- ReflectionProperty::IS_PRIVATE

```
$ref = new ReflectionClass($classname);
$properties = $ref->getProperties();
foreach($properties as $property){
    echo $property->getName();
}
//只获取私有属性
$properties = $ref->getProperties(ReflectionProperty::IS_PRIVATE);
foreach($properties as $property){
    echo $property->getName();
}
```

####获取类方法信息

```
$ref = new ReflectionClass($classname);
//参数
//ReflectionMethod::IS_STATIC
//ReflectionMethod::IS_FINAL
$methods = $ref->getMethods();
foreach($methods as $method){
    echo $method->getName();
}
$ref->hasMethod(string);  是否存在某个方法
//return bool
$ref->getMethod(string);  获取方法
//return object(ReflectionMethod)#2 (2) {
//  ["name"]=>
//  string(9) "getMethod"
//  ["class"]=>
//  string(15) "ReflectionClass"
//}
$ref->getMethod(string)->name; //获得方法名
$ref->getMethod(string)->class; //获得类名
```

###获取类的方法参数

```
$ref = new ReflectionClass($classname);
$ref->getMethod(string)->getParameters(); //获取方法的参数信息
//返回
[
    0 =>
    object(ReflectionParameter)[4]
      public 'name' => string 'test2'
]
```

####执行类的方法

```
class a{
    public function aname();
    static public function bname();
}
$aclass = new a();
//常规执行
$aclass->aname();                           // 执行Person 里的方法getName
// 或者：
$ref = new ReflectionClass($aclass);
$method = $ref->getMethod('aname');    // 获取a类中的getName方法
$method->invoke($aclass);                 // 执行getName方法
// 或者：
$ref = new ReflectionClass($aclass);
$method = $ref->getMethod('aname');     // 获取Person 类中的setName方法
$method->invokeArgs($aclass, array('snsgou.com'));
//静态方法的话
$ref = new ReflectionClass($aclass);
$ref->getMethod('bname')->invoke(null);
```

####获取类接口信息

```
$ref = new ReflectionClass($classname);
$interfaces = $ref->getInterfaces();
foreach($interfaces as $interface){
    echo $interface->getName();
}
```

###ReflectionMethod

```
$method = new ReflectionMethod('Person', 'test');
$method->isPublic();                //是否是公共方法
$method->isStatic();                //是否是静态方法
$method->getNumberOfParameters();   // 参数个数
$method->getParameters();           // 参数对象数组
```