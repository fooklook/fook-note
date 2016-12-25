##Nginx URL重写

Rewrite主要的功能就是实现URL的重写，Nginx的Rewrite规则采用PCRE Perl兼容正则表达式的语法规则匹配，如果需要Nginx的Rewrite功能，在编译Nginx之前，需要编译安装PCRE库。

通过Rewrite规则，可以实现规范的URL、根据变量来做URL转向及选择配置。

###相关指令

####if指令

语法：if(condition){…}

使用环境：server，location

该指令用于检查一个条件是否符合，如果条件符合，则执行大括号内的语句。if指令不支持嵌套，不支持多个条件&&和||处理。

其中，condition中可以包含的判断标识如下

- ~为区分大小写匹配
- ~*为不区分大小写匹配
- !~区分大小写不匹配
- !~*不区分大小写不匹配
- -f和!-f用来判断是否存在文件
- -d和!-d用来判断是否存在目录
- -e和!-e用来判断是否存在文件或目录
- -x和!-x用来判断文件是否可执行

例如，示例，如果是IE浏览器就进行跳转

```script
if ($http_user_agent ~MSIE){  
  rewrite ^(.*)$/msie/$1 break;  
}
```

####return指令

语法：return code

使用环境：server，location，if

该指令用于结束规则的执行并返回状态吗给客户端。状态码包括：

- 204（No Content）
- 400（Bad Request）
- 402（Payment Required）
- 403（Forbidden）
- 404（Not Found）
- 405（Method Not Allowed）
- 406（Not Acceptable）
- 408（Request Timeout）
- 410（Gone）
- 411（Length Required）
- 413（Request Entity Too Large）
- 416（Requested Range Not Satisfiable）
- 500（Internal Server Error）
- 501（Not Implemented）
- 502（Bad Gateway）
- 503（Service Unavailable）
- 504（Gateway Timeout）。

例如，示例，如果访问的URL以.sh .bash 结尾，返回状态码403

```script
location ~ .*\.(sh|bash)?$  
{  
  return 403;  
}
```

####set指令

语法：set variable value

使用环境：server，location，if

该指令用于定义一个变量，并给变量赋值。

####rewrite指令

语法：rewrite regex replacement flag

使用环境：server，location，if

该指令根据表达式来重定向URI，或者修改字符串。

flag标记有：

- last相当于Apache里的[L]标记，表示完成rewrite
- break终止匹配, 不再匹配后面的规则
- redirect返回302临时重定向 地址栏会显示跳转后的地址
- permanent返回301永久重定向 地址栏会显示跳转后的地址

例如，示例，将www重定向到http://

```script
if ($host ~* www\.(.*)){
  set $host_without_www $1;
  rewrite ^(.*)$ http://$host_without_www$1 permanent;
}
```

###使用案例

####域名永久重定向

```script
rewrite ^(.*)$  http://blog.720ui.com permanent;
```

####当访问的文件和目录不存在时，重定向到某个html文件

```script
if ( !-e $request_filename ){
  rewrite ^/(.*)$ error.html last;
}
```

####访问目录跳转

将访问/b跳转到/bbs目录上去

```script
rewrite ^/b/?$ /bbs permanent;
```

####目录对换

```script
/123456/xxxx  ====>   /xxxx?id=123456
rewrite ^/(d+)/(.+)/  /$2?id=$1 last;
```

####根据不同的浏览器将得到不同的结果

```script
if ($http_user_agent ~ Firefox) {  
   rewrite ^(.*)$ /firefox/$1 break;  
}
if ($http_user_agent ~ MSIE) {  
   rewrite ^(.*)$ /msie/$1 break;  
}
if ($http_user_agent ~ Chrome) {  
  rewrite ^(.*)$ /chrome/$1 break;  
}
```

####防止盗链

根据Referer信息防止盗链

```script
location ~*\.(gif|jpg|png|swf|flv)${  
  valid_referers none blocked www.cheng.com*.test.com;  
  if ($invalid_referer)  
    rewrite ^/(.*) http://www.lianggzone.com/error.html           
}
```

####禁止访问以/data开头的文件

```script
location ~ ^/data
{
  deny all;
}
```

####禁止访问以.sh,.exe为文件后缀名的文件

```script
location ~ .*\.(sh|exe)?$  
{  
  return 403;  
}
```

####设置某些类型文件的浏览器缓存时间

```script
location ~ .*.(gif|jpg|jpeg|png|bmp)$
{
  expires 30d;
}

location ~ .*.(js|css)$
{
  expires 1h;
}
```

####设置过期时间并不记录404错误日志

favicon.ico和robots.txt设置过期时间，为favicon.ico为99天,robots.txt为7天并不记录404错误日志。

```script
location ~(favicon.ico) {
  log_not_found off;
  expires 99d;
  break;
}
location ~(robots.txt) {
  log_not_found off;
  expires 7d;
  break;
}
```

####设置过期时间并不记录访问日志

设定某个文件的过期时间;这里为600秒，并不记录访问日志

```script
location ^~ /html/scripts/loadhead_1.js {
  access_log   off;
  root /opt/lampp/htdocs/web;
  expires 600;
break;
}
```