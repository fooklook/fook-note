##js函数表达式

###函数声明和函数表达式

```javascript
//函数声明
function functionName(arg0, arg1, arg2){}
//函数表达式
var functionName = function(arg0, arg1, arg2){}
```

####语法要点

```javascript
//函数先调用，后声明。这是合法的。
sayHi();
function sayHi(){
	alert('Hi');
}
//函数表达式，在使用前必须先赋值。
var sayHi = function(){
	alert('Hi');
};
sayHi();
```

因为**函数声明提升**，所以在代码执行前，会先读取函数声明。

####如何理解函数声明提升

```javascript
//这样做，在不同浏览器中的结果是不一样的，所以不建议这么做。
if(condition){
	function sayHi(){
		alert('Hi');
	}
}else{
	function sayHi(){
		alert('Yo');
	}
}
//但可以这样做。
if(condition){
	var sayHi = function(){
		alert('Hi');
	}
}else{
	var sayHi = function(){
		alert('Yo');
	}
}
```

因为函数声明会在代码执行前就被读取，所以程序并不知道condition这个变量的值，从而无法确定相关的值。而函数表达式，这是在代码执行时进行赋值，这样就能达到你预期的结果。

###递归

####习惯性递归的写法

```javascript
function factorial(num){
	if(num>1){
		return num * factorial(num-1);
	}else{
		return 1;
	}
}
```

但如果出现以下情况，就会出错。

```javascript
var Factorial1 = factorial;
fatorial = null;
alert(Factorial1(4));
```

合理的解决方法是:

```javascript
var factorial = (function f(num){
	if(num>1){
		return num * f(num-1);
	}else{
		return 1;
	}
});
```

###闭包

闭包是指有权访问另一个函数作用域中的变量的函数。

后台的每一个执行环境都会有一个变量的对象--变量对象。全局环境的变量对象始终存在着，而局部变量的变量对象，则只有在函数执行的过程中存在。而闭包中的匿名函数的作用链被赋给了全局变量，所以当函数执行完后，闭包中的匿名函数的活动对象没有被销毁。

####闭包作用域链的副作用

```javascript
function createFunction(){
	var result = new Array();
	for(var i=0;i<10;i++){
		result[i] = function(){
			return i;
		}
	}
}
//执行该函数，返回的结果对象，执行后，输出的结果都是10。
```

```javascript
//改为以下做法才能返回想要的结果。
function createFunctions(){
	var result = new Array();
	for (var i = 0; i <10; i++) {
		result[i] = function(num){
			return num;
		}(i);
	}
	return result;
}
```

