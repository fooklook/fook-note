##Nginx限速功能

如果很多用户，同一时刻下载nginx服务器上面的资源，这样会对nginx服务器的I/O产生极大负担，所以对nginx服务器的下载做限速设置很有必要。

首先在 http 段配置一个 limit_zone，然后在需要的地方使用 limit_conn 和 limit_rate 进行限速设置,如下一个简单的例子。

```script
http {
  limit_zone one $binary_remote_addr 10m;
  server {
    location /files/ {
      limit_conn one 1;
      limit_rate_after 1000k;
      limit_rate 100k;
    }
  }
}
```

- limit_zone，是针对每个IP定义一个存储session状态的容器。这个示例中定义了一个名叫one的10m大小的容器，这个名字会在后面的limit_conn中使用。
- limit_conn one 1，限制在one中记录状态的每个IP只能发起一个并发连接。
- limit_rate_after 1000k，在下载1000k后开始限速。
- limit_rate 100k，对每个连接限速100k. 注意，这里是对连接限速，而不是对IP限速。如果一个IP允许三个并发连接，那么这个IP就是限速为limit_rate×3，在设置的时候要根据自己的需要做设置调整，要不然会达不到自己希望的目的。
