## java常用代码

### 获取当前时间戳

```
//方法 一
System.currentTimeMillis();
//方法 二
Calendar.getInstance().getTimeInMillis();
//方法 三
new Date().getTime();
```

### 获取当前时间

```
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
String date = df.format(new Date());// new Date()为获取当前系统时间，也可使用当前时间戳
```

### 数组转集合

```
List<String> list = Arrays.asList(name);
```