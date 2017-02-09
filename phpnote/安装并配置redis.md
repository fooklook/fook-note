##安装并配置redis

###在ubuntu上安装redis

```shell
//下载 解压 安装
wget http://download.redis.io/releases/redis-3.2.6.tar.gz
tar xzf redis-3.2.6.tar.gz
cd redis-3.2.6
sudo make
```

###启动redis

再解压目录中启动redis

```shell
sudo src/redis-server
```

启动redis时，相当于redis的服务端，它是一个持续的状态。

###连接redis

这里需要打开一个新的终端，做为客户端连接redis，即使是本地连接。

```shell
sudo src/redis-cli
```

###命令行设置redis密码

```shell
//设置你想要的密码
config set requirepass password
```

密码设置成功后，你需要断开连接重新运行redis-cli

###通过密码连接redis

```shell
sudo src/redis-cli -h 127.0.0.1 -p 6379 -a password
```

###开启redis后台运行

修改redis.conf文件

```shell
daemonize yes
```

###关闭redis服务

```shell
pkill redis-server
//或者
redis-cli shutdown
```

###打开端口，供外网访问

```shell
//安装ufw
sudo apt-get install ufw
//开启端口6379
sudo ufw allow 6379
```