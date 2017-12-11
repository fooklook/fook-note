## php7+mysql5.7问题汇总

### mysql5.7安装及配置外网访问

```
//安装
sudo apt-get install mysql-server-5.7 mysql-client-5.7
//--修改root用户被外网登录--
//登录mysql
mysql -h localhost -u root -p
mysql > use mysql;
mysql > update user set host='%' where user='root';
mysql > flush privileges;
//授权用户
mysql > GRANT ALL ON *.* TO root@'%' IDENTIFIED BY 'userpassword' WITH GRANT OPTION;
mysql > flush privileges;
//修改配置文件
/etc/mysql/mysql.conf.d
注释：bind-address           = 127.0.0.1
```

### mysql5.7 timestamp特性

在mysql5.5以下版本，设置字段为timestamp类型时，可以通过设定字段"DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP"来实现，如果内容发生修改，则更新改字段时间为当前时间搓时间。

但是在mysql 5.6以上版本，如果设置字段为timestamp并且不为空，则字段会自动加上该属性，并且不可以修改。

