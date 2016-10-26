##Array对象

###实例化

```javascript
var myArray = new Array();

var myArray = new Array(2);       //这里指定数组长度，当数组长度少于指定是，后面将以undefined补充，超过时不会限定。
myArray[0] = "string";
console.log(myArray);               //["string",undefined]
myArray[1] = "string";
myArray[2] = "string";
console.log(myArray);               //["string","string","string"]

var myArray = new Array("hello");                    //一个元素的数组
var myArray = new Array("hello","hello","hello");    //三个元素的数组
```

Argument对象有同数组类似的行为，但它却不具有数组的一些方法。

###添加和删除元素

```javascript
//从尾部插入元素
var a = new Array();
a.push(1);                              //将数字1添加到尾部，[1]
a.push(2,3,4);                          //将数字2,3,4插入到尾部，[1,2,3,4]

a.pop();                                //将数组尾部的值弹出。[1,2,3]

a.unshift(1);                           //将数字1插入到数组头部，[1,1,2,3]
a.unshift(2,3,4);                       //将数字2,3,4插入到数组头部，[2,3,4,1,1,2,3]

a.shift();                              //将数组头部值弹出，[3,4,1,1,2,3]
```

//操作字符串

```javascript
var a =new Array(1,2,3);

//连接字符串
var s = a.join();                             //通过,将数组连接成字符串 1,2,3
s = a.join("|");                          //通过|将数字连接成字符串 1|2|3

//颠倒数组
a.reverse();                                //数组成[3,2,1]

//展开数组
//concat能展开一维数组，二维数组将无法展开
var arr = a.concat(1,2,3,[1,2,3],[1,[2,3]]);        //[3,2,1,1,2,3,1,2,3,1,[2,3]]

//获取数组指定片段
arr.slice(1,5);                             //[2,1,1,2,3]

//插入或者删除数组元素
var arr = new Array(1,2,3,4,5,6,7,8);
console.log(arr.splice(1));                     //[1]
console.log(arr);                               //[2,3,4,5,6,7,8]
console.log(arr.splice(1,2));                   //[3,4]
console.log(arr);                               //[2,5,6,7,8]
//删除部分数据，并插入部分数据
console.log(arr.splice(1,0,30,40,50,[1,2,3]));  //[5]
console.log(arr);                               //[2,30,40,50,[1,2,3],6,7,8]
```