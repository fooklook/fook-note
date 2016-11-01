##String对象

###获取字符串指定字符

```javascript
var str = "abcdefgc";
str.charAt(3);                      //返回d
str.charCodeAt(3);                  //返回d的Unicode编码
String.fromCharCode(100);           //从字符编码创建一个字符串。
String.fromCharCode(100，101,102);  //def
```

###获取字符所在位置

```javascript
str.indexOf("c");                   //返回2
str.lastIndexOf("c");               //返回7。
```

如果未找到对应字符，则返回-1

###截取字符串

```javascript
var str = "123456789";

str.slice(2,5);          //返回345
str.slice(2,-2);         //返回34567

str.substring(2,3)       //返回34
str.substring(3,2);      //返回34，该方法会先交换参数，再截取。
```

###连接拆分字符串

```javascript
var a = "12.34.56.789";
console.log(a.split(".", 3));            //[12,34,56]

a.concat("123");                          //连接字符串 12.34.56.789123
```

###大小写转换

```javascript
var str="Hello World!"
console.log(str.toLowerCase());                              //转换成小写
console.log(str.toUpperCase());                              //转换成大写
```

