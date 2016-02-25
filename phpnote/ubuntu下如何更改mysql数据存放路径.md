##ubuntu下如何更改mysql数据存放路径
###修改路径原因
数据库数据不断增加，但云服务器的硬盘费用是是非常贵的，所以我们可以将数据库文件存放在外挂硬盘上，这样可以通过及时更换大容量的外挂硬盘来平稳过度。
###设置方法
####设置新的存放路径
```shell
mkdir -p /data/mysql
```
####复制原有数据
```shell
cp -R /var/lib/mysql/* /data/mysql
```
####修改文件权限
```shell
chown -R mysql:mysql /data/mysql
```
这种权限修改方式我还是第一次见。
####修改配置
```shell
vim /etc/mysql/my.cnf
//找到datadir修改为指定路径
datadir = /data/mysql
```
####修改启动文件
```shell
vim /etc/apparmor.d/usr.sbin.mysqld
//将
/var/lib/mysql r,
/var/lib/mysql/** rwk,
//改为
/data/mysql r,
/data/mysql/** rwk,
```
####重启apparmor和mysql
```shell
sudo service apparmor restart
sudo service mysql restart 
```