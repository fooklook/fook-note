##js继承与多肽

###继承的三层含义

1. 子类的实例可以共享父类的方法。
2. 子类可以覆盖父类的方法或者扩展新的方法。
3. 子类和父类可以是子类实例的“类型”。

###构造继承法

通过call或apply的方法实现继承。

```javascript
var objectA = function(){
    this.userName = "name";                //这是对象属性，会被子类继承。
    this.fun = function(){                 //这是对象方法，会被子类继承。
        return 'objectA.fun';
    }
}
objectA.pUserName = 'pName';                //这是原型属性，子类无法继承。
objectA.prototype.pfun = function(){        //这是原型方法，子类无法继承。
    return 'objectA.pfun';
}
var objectB = function(){
    objectA.call(this);
}
var b = new objectB();
console.log(b instanceof objectA);          //false
```

通过以上继承法，继承的子类，原型属性和方法，子类无法调用。只能继承对象属性和方法。

构造继承法的优点是：

1. 简单直观，可以自由灵活用参数执行父类的够着函数。
2. 通过执行多个父类构造函数，实现多重继承。

缺点是：

1. 不能继承静态属性和方法。
2. 不能满足所有父类都是子类实例的类型，在实现多态时造成麻烦。
3. 继承关系不明，如b instanceof objectA，返回的是false。

###原型继承法

```javascript
var objectA = function(){
    this.userName = 'name';
    this.getName = function(){
        return this.userName;
    }
}
objectA.prototype.sex = 'man';
objectA.prototype.getSex = function(){
    return this.sex;
}
var objectB = function(){}
objectB.prototype = new objectA();
```

原型继承法的优点：

1. 解决了无法继承原型属性和方法的问题。
2. 结构简单。
3. 不需要复制父类的属性和方法，就能快速实现继承。

缺点：

1. 不支持多重继承。
2. 不能很好的支持多参数和动态参数的父类结构，因为在原型继承阶段你不能决定以什么参数来实例化父类对象。
3. 无法继承核心对象中，不可枚举的方法。
4. 副作用,如下：

```javascript
var objectA = function(){
    this.arr = new Array();
    this.getArr = function(){
        return this.userName;
    }
}
objectA.prototype.sex = 'man';
objectA.prototype.getSex = function(){
    return this.sex;
}
var objectB = function(){}
objectB.prototype = new objectA();
var a = new objectB();
var b = new objectB();
a.arr.push(1);
console.log(b.arr);                 //因为数组是引用类型，所以这里将输出[1]
```

###实例继承法

不管是构造继承法还是原型继承法，都无法充分的继承核心类。但是核心继承法，可以解决。

```javascript
var MyDate = function(){
    var instance = new Date();
    instance.printDate = function(){}
    return instance;
}
var myDate = new MyDate();
MyDate.printDate();
```

实例继承法的缺点：

1. 他需要在执行构造函数的时候，实例化核心类，这样就会对那些可以接受不同类型和不同数量参数的类型的继承赵成比较大的麻烦。
2. 该继承方法返回的是内部实例化的对象，而非对象本身，所以无法通过instanceof来判断。
3. 无法实现多重继承。

###拷贝继承法

枚举对象中的属性和方法，逐一进行拷贝，实现继承。

```javascript
Function.prototype.extends = function(obj){
    for(var each in obj){
        this.prototype.extends[each] = obj[each];
    }
}
var Point2D = function(){}
Point2D.extends(new Point());
```

拷贝继承法的缺点：

1. 不能拷贝无法枚举的方法。
2. 效率低。

###混合继承法

```javascript
var objectA = function(x, y){
    this.x = x;
    this.y = y;
}
var objectB = function(x, y, z){
    objectA.call(this, x, y);
    this.z = z;
}
objectB.prototype = new objectA();
```

又原型继承和构造继承组成的混合继承法，解决了原型继承法中的副作用，但是依然无法解决多重继承的问题。

###多肽

```javascript
var Animal = function(){
    this.bite = function(){
        return 'Animal bite';
    }
}
var Cat = function(){
    this.bite = function(){
        return 'Cat bite';
    }
}
Cat.prototype = new Animal();
var Dog = function(){
    this.bite = function(){
        return 'Dog bite';
    }
}
Dog.prototype = new Animal();
var AnimalBite = function(animal){
    if(animal instanceof Animal){
        animal.bite();
    }
}
var cat = new Cat();
var dog = new Dog();
AnimalBite(cat);
AnimalBite(dog);
```