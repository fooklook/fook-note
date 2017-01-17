##String对象

###获取字符串长度

```javascript
var str = "123456789";
str.length                  //返回9
```

###获取字符串指定字符

```javascript
var str = "abcdefgc";
str.charAt(3);                      //返回d
str.charCodeAt(3);                  //返回d的Unicode编码
str[1]                              //返回b  ES5支持

String.fromCharCode(100);           //从字符编码创建一个字符串。
String.fromCharCode(100，101,102);  //def
```

###获取字符所在位置

```javascript
var str = "abcdefghi";
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
str.substring(3,2);      //返回34，该方法会先交换参数，再截取，相当于substring(2,3)

str.substr(2)            //返回3456789
str.substr(2,3)          //f返回345
```

###连接拆分字符串

```javascript
var a = "12.34.56.789";
console.log(a.split(".", 3));            //[12,34,56]

a.concat("123");                          //连接字符串 12.34.56.789123
```

###字符串拼接

```javascript
var string = "abc";
var result = string.concat("defg");
result                              //abcdefg
var result1 = result.concat("defg", "hijklmn");
result1                             //abcdefghijklmn
```

###大小写转换

```javascript
var str="Hello World!"
console.log(str.toLowerCase());                              //转换成小写
console.log(str.toUpperCase());                              //转换成大写
```

###辅助函数

trim

```javascript
var str = "  abc  ";
var str1 = str.trim();
str1                    //abc 
```

###HTML方法

```javascript
var str = new String("hello");
str.anchor(name);                 //定义锚的名称<a name="name">hello</a>
str.big();                        //<big>hello</big>
str.blod();                       //<b>hello</b>
str.small();                      //<small>hello</small>
str.fixed();                      //<tt>hello</tt>
str.fontcolor("color");           //<font color="color">hello</font>
str.fontsize("size");             //<font size="size">hello</font>
str.italics();                    //<i>hello</i>
str.link("url");                  //<a href="url">hello</a>
str.strike();                     //删除线<strike>hello</strike>
str.sub();                        //下标<sub>hello</sub>
str.sup();                        //上标<sup>hello</sup>
```