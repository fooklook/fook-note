##js正则表达式

###常用正则表达式

```javascript
var Pattern = [];
Pattern.empty = /^[\s\n\r\t]*$/;                                 //匹配空字符
Pattern.RegInt = /^[0-9]*[1-9][0-9]*$/;                          //整数
Pattern.RegFloat = /^(+-)?(0|([1-9][0-9]*))([.][0-9]+)?$/;       //浮点型
//另一个浮点型/^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$/
Pattern.RegSPhone = /^[0-9]{6,8}([-][0-9]{1,6})?$/;              //短号码
Pattern.RegLPhone = /^[0-9]{3,4}[-][0-9]{6-8}([-][0-9]{1,6})?$/; //长号码
Pattern.RegCellPhone = /^1[34578]\d{9}$/;                        //手机号码
Pattern.RegEmail = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;//电子邮件
Pattern.RegURL = /^(http|https):\/\/([\W-]+\.)+[\w-]+(\/[\w-.\/?%&=])?$/; //网站地址
```

###常用特殊字符

- \w:任何ASCII单字符，等价于[a-zA-Z0-9]
- \W:任何非ASCII单字符，等价于[^a-zA-Z0-9]
- \s:任何Unicode空白符
- \S:任何非Unicode空白符
- \b:单词尾部
- \B:非单词尾部

###标志

- g:全文匹配
- i:忽略大小写
- m:多模式匹配，针对多行字符串，匹配每行中的内容（而不是把换行作为字符串的结束标志）。添加标志符m后，如果正则表达式中有^和$，则匹配每行的开始和结尾。

###属性

```javascript
var pattern = /abc./igm;
var text = "abcdefg";
pattern.source     //正则表达式正文 abc(.*)
pattern.flags      //正则表示修饰符 igm
pattern.global     //是否设置了g标志 true
pattern.ignoreCase //是否设置了i标志 true
pattern.multiline  //是否设置了m标志 true
pattern.lastIndex  //搜索下一个匹配项的字符位置 从零开始
var matches = pattern.exec(text);
pattern.lastIndex  //这里返回4
```

###exec

```javascript
var pattern = /abc(.)/g;
var text = "abcdefg";
var matches = pattern.exec(text);
//返回的数组属性
matches.input   //匹配的字符串  abcdefg
matches.index   //匹配的起始点  0
matches[0]      //匹配结果 abcd
matches[1]      //捕捉匹配的字符串 d
```

###test

判断是否匹配成功

```javascript
var pattern = /abc(.)/g;
var text = "abcdefg";
pattern.exec(text);     //返回 true
```

###search

search方法以正则表达式作为参数，返回第一个与之匹配的子串开始的位置，如果没有匹配则返回-1。

```javascript
var str = "abcdefghigklmn";
console.log(str.search(/efg/));               //4
```

###replace

replace方法执行检索与替换操作，第一个参数是正则表达式，第二个参数进行替换字符或者闭包。

```javascript
//普通匹配替换
var str = "just test";
var strReplace = str.replace(/(s)/g,"<span>$1</span>");
console.log(strReplace);

var exp = /(\d)([a-z])([A-Z])/g;
var str = "hehe呵呵1aZ呵呵2cS呵呵";
var fun = function(s,a,b,c){
	return a + s + b + c;
}
console.log(str.replace(exp, fun));       //hehe呵呵11aZaZ呵呵22cScS呵呵
```

###match

match方法只接受一个参数，正则表达式。

```javascript
var str = "just test";
var result = str.match(/(s)(t)/);
console.log(result);                 //["st", "s", "t"]

var str = "just test";
var result = str.match(/(s)(t)/g);
console.log(result);                 //["st", "st"] 匹配到两个
```

###split

```javascript
var str = "just test";
var result = str.split(/s/);
console.log(result);            //["ju", "t te", "t"]
```

