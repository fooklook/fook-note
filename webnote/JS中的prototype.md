##JS中的prototype

javascript的方法可以分为三类：

- 类方法
- 对象方法
- 原型方法

```javascript
function People(name)
{
    this.name=name;
    //对象方法
    this.Introduce=function(){
    alert("My name is "+this.name);
    }
}
//类方法
People.Run=function(){
    alert("I can run");
}
//原型方法
People.prototype.IntroduceChinese=function(){
    alert("我的名字是"+this.name);
}
//测试
var p1 = new People("Windking");
p1.Introduce();
People.Run();
p1.IntroduceChinese(); 
```

###prototype是什么含义

javascript中的每个对象都有prototype属性，Javascript中对象的prototype属性的解释是：返回对象类型原型的引用。

将一个对象设置为一个类型的原型，相当于通过实例化这个类型，为对象简历只读副本，在任何时候对副本进行改变，都不会影响到原始对象。而对原始对象进行改变，则会影响到每一个副本，除非改变属性已经被副本自己的同名属性覆盖。

```javascript
A.prototype = new B(); 
```

理解prototype不应把它和继承混淆。A的prototype为B的一个实例，可以理解A将B中的方法和属性全部克隆了一遍。A能使用B的方法和属性。这里强调的是克隆而不是继承。

```javascript
function baseClass(){
    this.showMsg = function(){
        alert("baseClass::showMsg");   
    }
}
function extendClass(){}
extendClass.prototype = new baseClass();
var instance = new extendClass();
instance.showMsg(); // 显示baseClass::showMsg
```

我们首先定义了baseClass类，然后我们要定义extentClass，但是我们打算以baseClass的一个实例为原型，来克隆的extendClass也同时包含showMsg这个对象方法。

extendClass.prototype = new baseClass()就可以阅读为：extendClass是以baseClass的一个实例为原型克隆创建的。

那么就会有一个问题，如果extendClass中本身包含有一个与baseClass的方法同名的方法会怎么样？

```javascript
function baseClass(){
    this.showMsg = function(){
        alert("baseClass::showMsg");   
    }
}
function extendClass(){
    this.showMsg =function (){
        alert("extendClass::showMsg");
    }
}
extendClass.prototype = new baseClass();
var instance = new extendClass();
instance.showMsg();//显示extendClass::showMsg
```

实验证明：函数运行时会先去本体的函数中去找，如果找到则运行，找不到则去prototype中寻找函数。或者可以理解为prototype不会克隆同名函数。

那么如果我想使用extendClass的一个实例instance调用baseClass的对象方法showMsg怎么办？

```javascript
extendClass.prototype = new baseClass();
var instance = new extendClass();
var baseinstance = new baseClass();
baseinstance.showMsg.call(instance);//显示baseClass::showMsg
```

这里的baseinstance.showMsg.call(instance);阅读为“将instance当做baseinstance来调用，调用它的对象方法showMsg”

好了，这里可能有人会问，为什么不用baseClass.showMsg.call(instance);

这就是对象方法和类方法的区别，我们想调用的是baseClass的对象方法。

最后尝试理解一下代码：

```javascript
function baseClass(){
    this.showMsg = function(){
        alert("baseClass::showMsg");   
    }
    this.baseShowMsg = function(){
        alert("baseClass::baseShowMsg");
    }
}
baseClass.showMsg = function(){
    alert("baseClass::showMsg static");
}
function extendClass(){
    this.showMsg =function ({
        alert("extendClass::showMsg");
    }
}
extendClass.showMsg = function(){
    alert("extendClass::showMsg static")
}
extendClass.prototype = new baseClass();
var instance = new extendClass();
instance.showMsg(); //显示extendClass::showMsg
instance.baseShowMsg(); //显示baseClass::baseShowMsg
instance.showMsg(); //显示extendClass::showMsg
baseClass.showMsg.call(instance);//显示baseClass::showMsg static
var baseinstance = new baseClass();
baseinstance.showMsg.call(instance);//显示baseClass::showMsg
```

###prototype的用法

####设置默认值

因为对象原型的值不会因副本变化而变化，所以为保证对象中有默认值，则可以这样使用。

```javascript
var objectA = function(){
    this.a = 100;
    this.b = 100;
    this.c = 100;
    //重置为默认值
    this.prototype.reset = function(){
        for(var each in this){
            delete this[each];
        }
    }
}
objectA.prototype = new objectA();
```

####使用prototype创建大量副本

创建大量复杂对象时，往往比较消耗资源。但可以通过prototype先以复杂对象为原型，然后通过创建新对象，则可以避免。

```javascript
var objectA = function(){
    //这是一个复杂对象
}
var objectB = function(){};             //这是一个空对象
objectB.prototype = new objectA();
var arr = Array();
for(var i=0; i<=100; i++){
    arr.push(new objectB());            //因为objectB是空对象，因此构造起来更加快。
}
```

####使用prototype定义方法可以减少创建对象时的系统开销。

```javascript
var objectA = function(){
    this.x = x;
    this.y = y;
}
objectA.prototype.fun1 = function(){};
```

上面的fun1可以通过this.fun1的方式声明，但通过上面的做法，可以减少在创建对象时，对fun1的赋值。