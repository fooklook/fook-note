##ES6语法

###基本语法

####let命令

它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

```javascript
{
  let a = 10;
  var b = 1;
}
a // ReferenceError: a is not defined.
b // 1
```

上面代码在代码块之中，分别用let和var声明了两个变量。然后在代码块之外调用这两个变量，结果let声明的变量报错，var声明的变量返回了正确的值。这表明，let声明的变量只在它所在的代码块有效。

for循环的计数器，就很合适使用let命令。

```javascript
for (let i = 0; i < 10; i++) {}
```

var与let的区别

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10

var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

####数组的解构赋值

```javascript
var [a, b, c] = [1, 2, 3];
//等同于var a = 1; var b = 2; var c = 3;
let [foo, [[bar], baz]] = [1, [[2], 3]];
//等同于var foo=1,bar=2,baz=3;
let [ , , third] = ["foo", "bar", "baz"];
//等同于var third="baz";
let [head, ...tail] = [1, 2, 3, 4];
//等同于var head=1,tail = [2, 3, 4];
let [x, y, ...z] = ['a'];
//等同于var x="a",var y=undefined,z=[];
```

####对象的解构赋值

```javascript
var { foo, bar } = { foo: "aaa", bar: "bbb" };
//等同于 var foo="aaa",bar="bbb";
var { bar, foo } = { foo: "aaa", bar: "bbb" };
//等同于 var foo="aaa",bar="bbb";
var { baz } = { foo: "aaa", bar: "bbb" };
//baz  undefined
let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
//f  'hello'
//l 'world'
//f和l才是需要被赋值的变量，first和last是模式。
var { foo: baz } = { foo: "aaa", bar: "bbb" };
//baz  "aaa"
//foo  error: foo is not defined
var obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};
var { p: [x, { y }] } = obj;
//x  "Hello"
//y  "World"
var {x:y = 3} = {};
//y  3
var {x:y = 3} = {x: 5};
//y  5
let { log, sin, cos } = Math;
//上面代码将Math对象的对数、正弦、余弦三个方法，赋值到对应的变量上
var arr = [1, 2, 3];
var {0 : first, [arr.length - 1] : last} = arr;
//first  1
//last   3
```

####字符串的解构赋值

```javascript
const [a, b, c, d, e] = 'hello';
//a  "h"
//b  "e"
//c  "l"
//d  "l"
//e  "o"
let {length : len} = 'hello';
//len  5
//字符串有一个length属性，所以可以将改属性赋值。
```

####数值和布尔值的解构赋值

```javascript
//解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
let {toString: s} = 123;
s === Number.prototype.toString // true
let {toString: s} = true;
s === Boolean.prototype.toString // true
//数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。

//由于undefined和null无法转为对象，所以无法发对它们进行解构赋值。
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

####函数参数的解构赋值

```javascript
function add([x, y]){
  return x + y;
}
add([1, 2]); // 3
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
function move({x = 0, y = 0} = {}) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```

####用途

#####交换变量的值

```javascript
[x, y] = [y, x];
```

上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。

#####从函数返回多个值

```javascript

function example() {
  return [1, 2, 3];
}
var [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
var { foo, bar } = example();
```

#####函数参数的定义

```javascript
function f([x, y, z]) { ... }
f([1, 2, 3]);
// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

#####提取JSON数据

```javascript
var jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};
let { id, status, data: number } = jsonData;
console.log(id, status, number);
// 42, "OK", [867, 5309]
```

#####函数参数的默认值

```javascript
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
}) {
  // ... do stuff
};
```

#####遍历Map结构

```javascript
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');
for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

#####输入模块的指定方法

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```