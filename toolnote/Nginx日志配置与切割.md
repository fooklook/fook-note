##Nginx日志配置与切割

访问日志主要记录客户端访问Nginx的每一个请求，格式可以自定义。通过访问日志，你可以得到用户地域来源、跳转来源、使用终端、某个URL访问量等相关信息。

Nginx中访问日志相关指令主要有两条，一条是log_format，用来设置日志的格式，另外一条是access_log，用来指定日志文职的存放路径、格式和缓存大小。两条指令在Nginx配置文件中的位置可以在http之间。

###日志配置

####log_format

log_format用来设置日志格式，格式如下所示

```script
log_format name(名称) format(格式)
```

在Nginx中有自己默认的日志格式，如下内容：

```script
#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
#                  '$status $body_bytes_sent "$http_referer" '
#                  '"$http_user_agent" "$http_x_forwarded_for"';
```

这段内容什么意思呢？我们来理解下。

- $remote_addr：客户端的ip地址（如果中间有代理服务器那么这里显示的ip就为代理服务器的ip地址）
- $remote_user：用于记录远程客户端的用户名称（一般为“-”）
- $time_local：用于记录访问时间和时区
- $request：用于记录请求的url以及请求方法
- $status：响应状态码
- $body_bytes_sent：给客户端发送的文件主体内容大小
- $http_referer：可以记录用户是从哪个链接访问过来的
- $http_user_agent：用户所使用的代理（一般为浏览器）
- $http_x_forwarded_for：可以记录客户端IP，通过代理服务器来记录客户端的ip地址

现在，我们添加一个我们自定义日志信息。例如，如果我们用Nginx作为反向代理服务，就不能获取客户端的真实IP地址IP了，因为经过反向代理后，在客户端和Web服务器之间增加了中间层，因此Web服务器无法直接拿到客户端的IP。

```script
log_format mylog '$remote_addr [$time_local] "$request" $status';
```

日志格式允许包含的变量注释，如下所示

```script
$remote_addr, $http_x_forwarded_for 记录客户端IP地址
$remote_user 记录客户端用户名称
$request 记录请求的URL和HTTP协议
$status 记录请求状态
$body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。
$bytes_sent 发送给客户端的总字节数。
$connection 连接的序列号。
$connection_requests 当前通过一个连接获得的请求数量。
$msec 日志写入时间。单位为秒，精度是毫秒。
$pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。
$http_referer 记录从哪个页面链接访问过来的
$http_user_agent 记录客户端浏览器相关信息
$request_length 请求的长度（包括请求行，请求头和请求正文）。
$request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
$time_iso8601 ISO8601标准格式下的本地时间。
$time_local 通用日志格式下的本地时间。
```

####access_log

用log_format指令设置了日志格式之后，需要用access_log指令指定日志文件存放路径。

格式如下所示

```script
access_log path(存放路径) [format(自定义日志格式名称) [buffer=size | off]]
```

在Nginx中有自己默认的日志路径，如下内容：

```script
#access_log  logs/access.log  main;
```

如果想关闭日志，可以如下：

```script
access_log off;
```

值得注意的是，Nginx进程设置的用户和组必须对日志路径有创建文件的权限，否则，会报错。

此外，对于每一条日志记录，都将是先打开文件，再写入日志，然后关闭。可以使用open_log_file_cache来设置日志文件缓存(默认是off)。

###日志切割

当网站访问量大后，日志数据就会很多，如果全部写到一个日志文件中去，文件会变得越来越大。文件大速度就会慢下来，比如一个文件几百兆。写入日志的时候，会影响操作速度。另外，如果我想看看访问日志，一个几百兆的文件，下载下来打开也很慢。

为了方便对日志进行分析计算，需要对日志进行定时切割。定时切割的方式有按照月切割、按天切割，按小时切割等。最常用的是按天切割。

####配置shell脚本

```script
#!/bin/bash
# 设置日志文件存放目录
logs_path="/var/logs/nginx/"
# 设置pid文件
pid_path="/usr/local/dev/nginx/nginx.pid"
# 重命名日志文件
mv ${logs_path}access.log ${logs_path}access_$(date -d "yesterday" +"%Y%m%d").log
# 向nginx主进程发信号重新打开日志
kill -USR1 `cat ${pid_path}`
```

####crontab中设置定时作业

进行编辑

```script
crontab -e
```

配置内容如下

```script
0 0 * * * bash /usr/local/dev/nginx/nginx_log.sh
```

这样在每天的夜晚12点就会自动创建备份文件了。