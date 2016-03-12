##在lnmp环境下安装laravel

###安装Nginx

```shell
//更新系统
sudo apt-get install update
//安装nginx服务
sudo apt-get install nginx
```

###安装mysql

```shell
sudo apt-get install mysql-server mysql-client
```

###安装php及必要扩展

```shell
//安装php
sudo apt-get install php5 php5-cli php5-fpm php5-mysql
//安装mcrypt扩展，安装laravel项目必备扩展
sudo apt-get install php5-mcrypt php5-dev
```

###配置nginx使其使用php-fpm进程

```shell
//备份/etc/nginx/sites-available/default文件
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.back
```

将default文件修改为一下内容

```php
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        root /usr/share/nginx/html;
        index index.html index.htm index.php;

        # Make site accessible from http://localhost/
        server_name localhost;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }

        # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
        #location /RequestDenied {
        #       proxy_pass http://127.0.0.1:8080;
        #}

        error_page 404 /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
                root /usr/share/nginx/html;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini

                # With php5-cgi alone:
                #fastcgi_pass 127.0.0.1:9000;
                # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}

```

###安装CLI

```shell
sudo apt-get install cli
```

###安装composer包

```shell
//下载composer
curl -sS https://getcomposer.org/installer | php
//设置全局命令
sudo mv composer.phar /usr/local/bin/composer
```

###创建laravel项目

```shell
//创建一个laravel项目
sudo composer create-project laravel/laravel laravel --prefer-dist
//安装完成后，修改vendor、bootstrap、storage目录的权限
sudo chmod -R 777 vendor/
sudo chmod -R 777 bootstrap/
sudo chmod -R 777 storage/
```

###配置nginx，实现rewrite功能

```shell
//将/etc/nginx/sites-available/default中的location内容改为如下
location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
                try_files $uri $uri/ /index.php?$query_string;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
```

###为网站配置域名

```php
//在/etc/nginx/sites-available/default文件后面添加一下内容
server {
        listen 80;

        root /usr/share/nginx/html/laravel/public;
        index index.html index.htm index.php;

        # Make site accessible from http://localhost/
        server_name testnginx.com;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
                try_files $uri $uri/ /index.php?$query_string;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }

        # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
        #location /RequestDenied {
        #       proxy_pass http://127.0.0.1:8080;
        #}

        error_page 404 /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
                root /usr/share/nginx/html;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini

                # With php5-cgi alone:
                #fastcgi_pass 127.0.0.1:9000;
                # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}
```