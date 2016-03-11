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
```