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
3. 副作用,如下：

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