## mysql服务器搭建日志

### 安装mysql

```
sudo apt-get update
sudo apt-get install mysql-server
```

中间需要输入root账号密码

### 授权用户远程登录

#### 在服务器上以root用户身份登录mysql

```
mysql -h localhost -u root -p
```

#### 授权mysql用户远程访问

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypasssword' WITH GRANT OPTION;
//刷新缓存
FLUSH PRIVILEGES;
```

#### 修改mysql配置,允许远程ip访问

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

添加'#'注释掉其中的"bind-address = 127.0.0.1"

#### 重启mysql

```
service mysql restart
```