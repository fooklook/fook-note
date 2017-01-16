##js遍历函数

###filter

对数组中的每一项运行给定的函数，返回该函数会返回true的项组成的数组。

```javascript
function isBigEnough(element, index, array) {
    return (element >= 10);
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
//结果12, 130, 44
```

###map

对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

```javascript
var strings = ["hello", "Array", "WORLD"];
function makeUpperCase(v)
{
    return v.toUpperCase();
}
var uppers = strings.map(makeUpperCase);
//结果 ["HELLO", "ARRAY", "WORLD"]
```

###some

对数组中的每个元素都执行一次指定的函数（callback），直到此函数返回 true，如果发现这个元素，some 将返回 true，如果回调函数对每个元素执行后都返回 false ，some 将返回 false。

```javascript
function isBigEnough(element, index, array) {
    return (element >= 10);
}
var passed = [2, 5, 8, 1, 4].some(isBigEnough);
// passed is false
passed = [12, 5, 8, 1, 4].some(isBigEnough);
// passed is true
```

###every

对数组中的每个元素都执行一次指定的函数（callback），直到此函数返回 false，如果发现这个元素，every 将返回 false，如果回调函数对每个元素执行后都返回 true ，every 将返回 true。

```javascript

function isBigEnough(element, index, array) {
    return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// passed is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true
```

###forEach

对数组中的每一项运行给定的函数，这个方法没有返回值。

```javascript
function printElt(element, index, array) {
    document.writeln("[" + index + "] is " + element + "<br />");
}
[2, 5, 9].forEach(printElt);
// Prints:
// [0] is 2
// [1] is 5
// [2] is 9
```

###lastIndexOf

比较 searchElement 和数组的每个元素是否绝对一致（===），当有元素符合条件时，返回当前元素的索引。如果没有发现，就直接返回 -1 。

```javascript
var array = [2, 5, 9, 2];
var index = array.lastIndexOf(2);
// index is 3
index = array.lastIndexOf(7);
// index is -1
index = array.lastIndexOf(2, 3);
// index is 3
index = array.lastIndexOf(2, 2);
// index is 0
index = array.lastIndexOf(2, -2);
// index is 0
index = array.lastIndexOf(2, -1);
// index is 3
```

###indexOf

功能与lastIndexOf()一样，搜索是正向进行的

```javascript
var array = [2, 5, 9];
var index = array.indexOf(2);
// index is 0
index = array.indexOf(7);
// index is -1
```