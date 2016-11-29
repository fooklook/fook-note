##js闭包

闭包：是指有权访问另外一个函数作用域中的变量的函数。创建闭包的常见方式就是在一个函数内部创建另外一个函数。

在javascript中没有块级作用域，一般为了给某个函数申明一些只有该函数才能使用的局部变量时，我们就会用到闭包，这样我们可以很大程度上减少全局作用域中的变量，净化全局作用域。

###自我总结

闭包，首先内部环境对外部不可见，也就是说闭包具有控制外部域的能力但是又能防止外部域对闭包环境的污染。

闭包最大的特点是不需要通过传递变量的方式就可以从内层直接访问外层环境，这位多层嵌套下的函数式程序带来了极大的便利。

```javascript
var data = [];
for (var k = 0; k < 3; k++) {
    data[k] = function () {
        alert(k);
    };
}
data[0](); // 3, 而不是0
data[1](); // 3, 而不是1
data[2](); // 3, 而不是2

//一目明了，使用闭包之后

var data = [];
for (var k = 0; k < 3; k++) {
    data[k] = (function _helper(x) {
        return function () {
            alert(x);
        };
    })(k); // 传入"k"值
}
// 现在结果是正确的了
data[0](); // 0
data[1](); // 1
data[2](); // 2
```

###使用闭包的注意点

- 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
- 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

###例子
代码实例1：

```javascript
    function f1(){

　　　　var n=999;

　　　　nAdd=function(){n+=1}

　　　　function f2(){
　　　　　　alert(n);
　　　　}

　　　　return f2;

　　}

　　var result=f1();

　　result(); // 999

　　nAdd();

　　result(); // 1000
```
在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。

代码实例2：

```javascript
    var name = "The Window";
　　var object = {
　　　　name : "My Object",

　　　　getNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};

　　　　}

　　};
　　alert(object.getNameFunc()());
//输入The Window
    var name = "The Window";

　　var object = {
　　　　name : "My Object",

　　　　getNameFunc : function(){
　　　　　　var that = this;
　　　　　　return function(){
　　　　　　　　return that.name;
　　　　　　};

　　　　}

　　};

　　alert(object.getNameFunc()()); 
//输出My object
```

###闭包中概念的理解

####私有域

```javascript
var a,b;
(function(){
    showAB = function(){        //未进行var初始化，showAB为全局函数
        console.log(a+"--"+b);
    }
    var a=10,b=20;
})();
a = -10;
b = -20;
console.log(a+"--"+b);
showAB();
```

####观察者

```javascript
function ClassA(){
    var a = 100;
    var b = 200;
    this.declareFriend = function(fritend){
        fritend.getPrivateProperty = function(privateName){
            return eval(privateName);
        }
    }
}
var objA = new ClassA();
var objB = new Object();
objA.declareFriend(objB);
objB.getPrivateProperty("a");
objB.getPrivateProperty("b");
```