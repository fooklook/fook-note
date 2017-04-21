##html5应用缓存

###配置web服务器支持应用缓存

Apache服务器开启支持的方式是：在conf/mime.types中添加一段代码：

> test/cache-manifest   manifest

IIS服务器开启方式：

右键--HTTP---MIME映射下，单击文件类型---新建---扩展名输入manifest，类型中输入test/cache-manifest

nginx服务器开启支持的方式是：在conf/mime.types中添加一段代码：

> text/cache-manifest   manifest

###Manifest 文件

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

manifest 文件可分为三个部分：

* CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
* NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
* FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

####实例 - 完整的 Manifest 文件

```
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.php

FALLBACK:
/html/ /offline.html
```

以 "#" 开头的是注释行，但也可满足其他用途。更新注释行中的日期和版本号是一种使浏览器重新缓存文件的办法。

引入manifest的页面,即使没有被列入缓存清单中，仍然会被用户代理缓存。

###html5应用缓存的使用

通过FALLBACK指定文件返回失败（断网或者404）时，返回统一的代替文件。

```html
<!DOCTYPE html>
<html lang="en" manifest="index.appcache">
<head>
	<meta charset="UTF-8">
	<title>html5应用缓存</title>
</head>
<body>
	<img src="images/001.png">
	<img src="images/001.png">
</body>
</html>
```

```
CACHE MANIFEST
#v06
images/01.png

NETWORK:
*

FALLBACK:
/images/ /images/002.png
```

当images/001.png图片在断网情况下，或者服务器不存在001.png图片的情况下，会使用/images/002.png代替。在客户端层面解决了文件指向问题。