##js实用函数
###数组转字符串

```javascript
var colors = ['red','blue','green'];
console.log(colors.toString());
//log输出：red,blue,green
```

###数组栈方法

```javascript
var color = new Array();
color.push('red','black');	//在数组末尾逐个添加,并返回数组的长度
//['red','black']
var black = color.pop();	//从数组中取出最后一项，并返回最后一项的值。
//['red']
```

###数组队列方法

```javascript
var color = new Array();
color.push('red','black');	//在数组末尾逐个添加,并返回数组的长度
//['red','black']
var black = color.unsify();	//从数组中取出第一项,并返回第一项的值
//['black']
```

####相反方向模拟队列

```javascript
var color = new Array();
color.unshift("red","black");		//在数组的前端逐个添加，并返回数组的长度。
//['black','red']
var red = color.pop();
['red']
```

###数组重新排序

```javascript
var array = [1,2,3,4,5,6,10];
array.reverse();		//数组反向排序
//[10,6,5,4,3,2,1]
array.sort();			//按字符串比较，升序排序。
//[1,10,2,3,4,5,6]
//对数值数组按自然升序排序
function compare(value1,value2){
	return value2 - value1;
}
array.sort(compare);
```

###数组相关操作

```javascript
//concat基于当前数组创建新的数组
var colors = ['red','green','blue'];
var colors2 = colors.concat('yellow',['black','brown']);
//colors2 -> red,green,blue,yellow,black,brown
//slice取出数组中的多个元素，创建一个新的数组
var colors = ["red","green","blue","yellow","purple"];
var color2 = colors.slice(1);
//[green,blue,yellow,purple]
var color3 = colors.slice(1,4);
//green,blue,yellow
//splice方法
var colors = ['red','green','blue'];
var removed = colors.splice(0,1);	//删除第一项
//colors -> green,blue
var removed = colors.splice(1,0,'yellow','orange');
//colors -> green,yellow,orange,blue
var removed = colors.splice(1,1,'red','purple');
//colors -> green,red,purple,orange,blue
```

###迭代方法
every():对数组中的每一项运行都给定函数，如果该函数对每一项都返回true，则返回true。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = number.enery(function(item,index,array)){
	return (item>2);
}
console.log(everyResult);	//false
```

some():对数组中的每一项运行给定的函数，如果函数对任何一项返回true，则返回true。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var someResult = numbers.some(function(item,index,array){
	return (item>2);
});
console.log(someResult);	//true
```

filter():对数组中的每一项运行给定的函数，返回该函数会返回true的项组成的数组。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item,index,array){
	return (item>2);
});
console.log(filterResult);	//[3,4,5,4,3]
```

map():对数组中的每一项运行给定的函数，返回每次函数调用的结果组成的数组。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var mapResult = numbers.map(function(item,index,array){
	return (item * 2);
});
console.log(mapResult);	//[2,4,6,8,10,8,6,4,2]
```

forEach():对数组中的每一项运行给定函数。没有返回值。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item,index,array){
	//执行某些操作
});
```

reduce():从数组中的第一项开始，逐个遍历到最后一项。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var sum = numbers.reduce(function(prev,cur,index,array){
	return prev + cur;
});
//sum -> 15
```

reduceRight():从数组中的最后一项开始，逐个遍历到第一项。

```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var sum = numbers.reduceRight(function(prev,cur,index,array){
	return prev + cur;
});
//sum -> 15
```

字符串处理函数

```javascript
var s1 = "some text";
s1.length	//9
s1.charAt(1)	//o  以字符串形式，返回特定位置的字符
s1.charCodeAt(1)	//101  返回字符编码
s1.slice(-3);		//"ext"
sl.substring(-3);	//"some text"
sl.substr(-3);		//ext
s1.slice(3,-4);		//"e "
s1.substring(3,-4);	//"som"
s1.substr(3,-4);	//""
//字符串所在位置
var string = "hello world";
string.indexOf("o");	//4
string,indexOf("o",6);	//7
string.lastIndexOf("o");	//7
string.lastIndesOf("o",6);	//4
//字符串转数字
parseInt("100abc");
parseFloat("1000.01")// 1000.01 取浮点型
Math.floor("1000.01"); // 1000向下取整
var x = Math.round("1000.56"); // 1001 四舍五入
//trim
var string = "   hello  ";
string.trim();		//hello
//大小写转换
var string = "hello world";
string.toLowerCase();		//"HELLO WORLD"
string.toUpperCase();		//"hello world"‘
//正则匹配
var text = 'cat,bat,sat,fat';
var pattern = /.at/;
var matches = text.match(pattern);
matches.index	//0
matches[0]	//"cat"
parrern.lastIndex	//0
//匹配替换
var result = text.replace("at","ond");
result					//"cond,bond,sond,fond"
var result = text.replace(/at/g,"ond");
result					//"cond,bond,sond,fond"
var result = text.replace(/(.at)/g,"world ($1)");
result					//world (cat),world (bat), world(sat),world(fat)
```

数字处理函数

```javascript
var num = 10;
num.toString(); //"10"	取字符串
num.toString(0); //"1010" 取2进制
num.Fixed(2);	//"10.00" 浮点型
//舍入方法
Math.ceil(25.9);	//26 向上取整
Math.round(25.9);	//26 四舍五入
Math.floor(25.9);	//向下取整
//生成随机数
Math.random()		//生成0-1的小数
```
