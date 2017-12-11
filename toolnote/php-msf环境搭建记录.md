## php-msf环境搭建记录

### 安装环境

- gcc 4.4以上版本
- PHP 7.0以上
- swoole 2.0以下 1.9.15以上
- mysql 5.6以上
- composer
- redis
- yac

### 需要安装的php扩展

可以通过pecl命令进行安装，并且需要修改php.ini文件。

- inotify
- swoole
- redis

composer运行时，需要对当前php版本进行检测，php7的版本过高，所以在执行composer install时会出现Your requirements could not be resolved to an installable set of packages. 

所以运行composer install --ignore-platform-reqs或者composer update --ignore-platform-reqs。

### 安装自动创建项目模板

- php -r "copy('https://raw.githubusercontent.com/pinguo/php-msf-docker/master/installer.php', 'installer.php');include('installer.php');" && source ~/.bashrc
- php installer.php
- 按照提示输入配置
- 进入项目目录，通过composer install --ignore-platform-reqs下载依赖。

### 启动服务

#### 调试模式

```
./server.php start
```

#### Daemon模式

```
./server.php start -d
```

#### 停止服务

```
./server.php stop
```

#### 重启服务

```
- ./server.php restart
```
