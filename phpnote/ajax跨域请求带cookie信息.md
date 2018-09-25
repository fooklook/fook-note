## ajax跨域请求带cookie信息

### 同源策略

同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。

同源策略是处于对用户安全的考虑，如果非同源就会受到以下限制：

1. cookie不能读取
2. dom无法获得
3. ajax请求不能发送

### CORS

CORS(跨域资源共享)是由W3C制定的跨站资源分享标准，可以让AJAX实现跨域访问。想要了解跨域的话，首先需要了解下简单请求：


请求方式为GET或者POST

假若请求是POST的话，Content-Type必须为下列之一：

1. application/x-www-form-urlencoded
2. multipart/form-data
3. text/plain

不含有自定义头（类似于segmentfault自定义的头X-Hit）

#### CORS工作原理

CORS实现跨域访问并不是一蹴而就的，需要借助浏览器的支持，从原理题图我们可以清楚看到，简单的请求（通常指GET/POST/HEAD方式，并没有去增加额外的请求头信息）直接创建了跨域请求的XHR对象，而复杂的请求则要求先发送一个”预检”请求，待服务器批准后才能真正发起跨域访问请求。

![CORS工作原理](CORS0001.png)

#### Request Headers（请求头）

Origin

表示跨域请求的原始域。

Access-Control-Request-Method

表示跨域请求的方式。（如GET/POST）

Access-Control-Request-Headers

表示跨域请求的请求头信息。

#### 服务端配置项解析

1. Access-Control-Allow-Origin
该字段必填。它的值要么是请求时Origin字段的具体值，要么是一个*，表示接受任意域名的请求。
2. Access-Control-Allow-Methods
该字段必填。它的值是逗号分隔的一个具体的字符串或者*，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。
3. Access-Control-Expose-Headers
该字段可选。CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。
4. Access-Control-Allow-Credentials
该字段可选。它的值是一个布尔值，表示是否允许发送Cookie.默认情况下，不发生Cookie，即：false。对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json，这个值只能设为true。如果服务器不要浏览器发送Cookie，删除该字段即可。
5. Access-Control-Max-Age
该字段可选，用来指定本次预检请求的有效期，单位为秒。在有效期间，不用发出另一条预检请求。


### 代码实例

本实例实现，在www.a.com域名下登录。然后在test.oa.com域名下

#### 客户端代码

```html
//test.oa.com域名代码
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>test</title>
	<script type="text/javascript" src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
</head>
<body>
<script type="text/javascript">
	$.ajax({
    type: "POST",
    url: "http://www.a.com/get.php",
　　　　// 允许携带证书
    xhrFields: {
       withCredentials: true
    },
　　　　// 允许跨域
    crossDomain: true,
    success:function(result){
    },
    error:function(){
  }
})
</script>
</body>
</html>
```

#### 服务端代码

```php
//www.a.com域名下的代码
//login.php 设置用户登录态和session信息
session_name("a_session");          //手动设置session_name方便后面设置域名共享
session_start();
setcookie("a_session", session_id(),time()+3600*24,"/",".a.com");
echo $_SESSION['user']=$_GET['user'];
//get.php 读取服务端session信息
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods", "*");
header("Access-Control-Max-Age", "3600");
header("Access-Control-Allow-Headers", "DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization,SessionToken");
header("Access-Control-Allow-Credentials", "true");
session_name("a_session");
session_start();
if(isset($_SESSION['user'])){
	echo "alert({$_SESSION['user']})";
}else{
	echo "false";
}
```

