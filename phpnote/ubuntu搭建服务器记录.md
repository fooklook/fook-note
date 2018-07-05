## ubuntu搭建服务器记录
服务器安装的是ubuntu server14.01版本，没有图形化见面，所以所有的操作都是通过命令行实现。

实现目标：

- ssh远程访问
- 搭建lamp环境
- 开启mysql远程连接
- 安装composer
- 安装xunsearch，并开启xunsearch服务
- 安装git版本控制器

### ssh远程访问
很多时候我们不方便直接在服务器上进行操作，比如服务器安装后终端界面显示的中文为乱码，当然可以进行修复，但是可以用更简单的方法解决这个问题，就是通过ssh远程登录客户端。在windows下不管用git-bash或者putty工具，都不会出现乱码的情况。

```shell
sudo apt-get install openssh-client openssh-server
ssh服务启动：sudo /etc/init.d/ssh start
ssh停止服务：sudo /etc/init.d/ssh stop
ssh重启服务：sudo /etc/init.d/ssh restart
```

### 搭建lamp环境
使用最简单除暴的方式安装

```shell
sudo apt-get install apache2 mysql-server mysql-client php5 php5-gd php5-mysql
//安装 mcrypt 扩展
sudo apt-get install php5-mcrypt php5-dev
//开启 Mcrypt 模块
sudo php5enmod mcrypt
//开启 Mod_rewrite 模块
sudo a2enmod rewrite
//重启apache2服务器
sudo service apache2 restart
```

### 开启mysql远程访问

```shell
vim /etc/mysql/my.cnf找到bind-address = 127.0.0.1
```

注释掉这行，如：#bind-address = 127.0.0.1  
或者改为： bind-address = 0.0.0.0

```shell
//重启mysql
sudo service mysql restart
```

通过ssh远程连接，登录到mysql客户端

```mysql
mysql -h localhost -u root -p
//--》输入数据库密码
//授权用户能进行远程连接
grant all privileges on *.* to root@"%" identified by "password" with grant option;
flush privileges;
```
第一行命令解释如下，\*.\*：第一个\*代表数据库名；第二个\*代表表名。这里的意思是所有数据库里的所有表都授权给用户。root：授予root账号。“%”：表示授权的用户IP可以指定，这里代表任意的IP地址都能访问MySQL数据库。“password”：分配账号对应的密码，这里密码自己替换成你的mysql root帐号密码。
### 安装composer
下载composer包

```shell
curl -sS https://getcomposer.org/installer | php
```
设置全局命令
```shell
sudo mv composer.phar /usr/local/bin/composer
```

### 安装xunsearch，启动，并将xunsearch加入到开机启动
首先要确保ubuntu安装了gcc g++ make

```shell
sudo apt-get install make gcc g++
```
然后安装zlib，用来解压的：

```shell
sudo apt-get install zlib1g-dev 
```
下载和安装
```shell
//下载
wget http://www.xunsearch.com/download/xunsearch-full-latest.tar.bz2
//解压在当前目录
tar -xjf xunsearch-full-latest.tar.bz2
//进入解压目录
cd xunsearch-full-1.3.0
//安装
sh setup.sh
//根据提示输入xunsearch安装目录，建议安装在/usr/local目录下。等待安装...
```
开启xunsearch  
$prefix/bin/xs-ctl.sh start  
绑定全部IP，适合SDK/服务端 不同服务器的情况  
$prefix/bin/xs-ctl.sh -b inet start  
停止服务器，若启动时指定了-b inet此处也必须指定  
$prefix/bin/xs-ctl.sh stop 

将xunsearch加入到开机启动中

```shell
sudo vim /etc/rc.local
```
在exit 0前面加入启动命令

xunsearch是通过8383和8384两个端口执行相关动作的，如果要想它们被外网访问，就要开启端口。

安装防火墙并开启8383和8384端口

```shell
sudo apt-get install ufw
ufw allow 8384
ufw allow 8383
```

### 安装git
```shell
sudo apt-get install git
```