## https服务搭建

### 申请ssl证书

腾讯云提供[域名型免费版(DV)](https://buy.cloud.tencent.com/ssl)申请。

但不清楚是否必须购买服务器才能提供申请服务。

### 获取记录值

提交申请后，会自动跳转到证书列表，点击查看详情，获取“主机记录”和“记录值”。

### 配置记录值

不管是哪里注册的域名，配置域名解析，将主机记录和记录值，配置到域名解析记录中。

### 等待验证

回到证书列表，点击验证。

### 下载证书

验证完成后，过几分钟，在列表页的状态，就会自动变成已颁发。在操作栏就变成下载项。

### 配置服务器

```
server { 
        listen 80; 
        server_name localhost; 
        location / { 
                try_files $uri $uri/ =404; 
        } 
        # 设置http访问时自动重定向至https 
        rewrite ^ https://$http_host$request_uri? permanent; 
}
server {
        # SSL configuration
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        ssl                  on;
        ssl_certificate      xxxxx.crt;
        ssl_certificate_key  xxxxx.key;
        ssl_session_timeout 5m;
        ssl_prefer_server_ciphers on;
        ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;

        root /var/www/html/easyshow-admin/public;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.php;

        server_name server_name;

        error_page  404              /404.html;

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
                root /var/www/html;
        }

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/var/run/php/php5.6-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
                include fastcgi_params;

        }
}
```

