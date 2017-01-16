##Date对象

```javascript
Date();                            //获取当前时间
var objectDate = new Date();
objectDate.toString();             //当前时间
objectDate.getDate();              //返回指定日期中的某一天（1~31）
objectDate.getDay();               //返回指定日期中的周几（0~6）
objectDate.getMonth();             //返回日期中的月份 (0 ~ 11)
objectDate.getFullYear();          //从 Date 对象以四位数字返回年份
objectDate.getHours();              //返回 Date 对象的小时 (0 ~ 23)
objectDate.getMinutes();            //返回 Date 对象的分钟 (0 ~ 59)
objectDate.getSeconds();            //返回 Date 对象的秒数 (0 ~ 59)
objectDate.getMilliseconds();       //返回 Date 对象的毫秒(0 ~ 999)
objectDate.getTime();               //返回 1970 年 1 月 1 日至今的毫秒数
Date.parse(string);                 //解析一个日期时间字符串，并返回 1970/1/1 午夜距离该日期时间的毫秒数              
objectDate.setDate(int);            //用于设置一个月的某一天(1 ~ 31)
objectDate.setMonth(int);           //设置 Date 对象中月份 (0 ~ 11)
objectDate.setFullYear(int);        //设置 Date 对象中的年份（四位数字)
objectDate.setHours();              //设置 Date 对象中的小时 (0 ~ 23)
objectDate.setMinutes();            //设置 Date 对象中的分钟 (0 ~ 59)
objectDate.setSeconds();            //设置 Date 对象中的秒钟 (0 ~ 59)
objectDate.setMilliseconds();       //设置 Date 对象中的毫秒 (0 ~ 999)
objectDate.setTime();               //以毫秒设置 Date 对象。

objectDate.toTimeString();          //把 Date 对象的时间部分转换为字符串
objectDate.toDateString();          //把 Date 对象的日期部分转换为字符串
objectDate.toLocaleString();        //根据本地时间格式，把 Date 对象转换为字符串
objectDate.toLocaleTimeString();    //根据本地时间格式，把 Date 对象的时间部分转换为字符串
objectDate.toLocaleDateString();    //根据本地时间格式，把 Date 对象的日期部分转换为字符串
```

###使用

var myDate = new Date();            //创建Date对象时，对象会自动把当前日期和时间保存为参数形式。

同时可以传入以下五种参数：

```javascript
new Date("month dd,yyyy hh:mm:ss");
new Date("month dd,yyy");
new Date(yyyy,mth,dd,hh,mm,ss);
new Date(yyy,mth,dd);
new Date(时间戳);
```