##traits
php没有多继承，使得代码复用性降低。但是在php5.4以后引入了traits，实现代码的复用机制。

trait的生明和类相似，但它不能被实例化。
###基本语法
```php
trait name(){
    private $name;
    public function getname(){retrn $this->name;}
    private function setname($name){$this->name = $name;}
}
trait tel(){}
class people extends animal{
    use name,use;
}
```
###优先级
如果在trait中和class中都声明了同一个属性或则方法，那么应该以类中属性和方法为准。

###引用多个trait,存在同名方法
为了解决多个 trait 在同一个类中的命名冲突，需要使用 insteadof 操作符来明确指定使用冲突方法中的哪一个。

或者通过as为方法设定别名。

```php
trait A{
    public function name();
}
trait B{
    public function name();
}
class C{
    use A,B{
        B::name insteadof A;//B中的name代替A中的name,也就是使用B中的name
	A::name as aname;
    }
}
```
