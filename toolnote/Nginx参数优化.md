##Nginx参数优化

###本机缓存设置

浏览器缓存是为了提高加载速度，因此我们可以通过Nginx对静态文件进行缓存。

```script
location ~ ^/(images|javascript|js|css|flash|media|static)/ {
    #过期30天
    expires 30d;
}
```

###定义错误提示页面

```script
error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        root   html;
}
```

###自动显示目录

```script
location / {
    autoindex on;
}
```

此外，还可以添加两个参数

- autoindex_exact_size off; 默认为on，显示出文件的确切大小，单位是bytes。改为off后，显示出文件的大概大小，单位是kB或者MB或者GB
- autoindex_localtime on; 默认为off，显示的文件时间为GMT时间，改为on后，显示的文件时间为文件的服务器时间
