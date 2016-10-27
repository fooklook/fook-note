##mysql添加删除用户
###直接操作mysql库
```mysql
//使用mysql库
use mysql
//新增用户
insert into mysql.user(Host,User,Password) values("localhost","lionbule",password("hello1234"));
flush privileges;//将mysql表中的用户信息提取到内存里
//修改用户密码
update mysql.user set password=password('new password') where User="lionbule" and Host="localhost";
flush privileges;
//修改用户名
rename user username to newusername; 
//删除用户
DELETE FROM user WHERE User="lionbule" and Host="localhost";
flush privileges;
```

###修改用户权限
####grant用法
```mysql
权限：
    常用总结, ALL/ALTER/CREATE/DROP/SELECT/UPDATE/DELETE
数据库：
*.*                    表示所有库的所有表
test.*                表示test库的所有表
test.test_table  表示test库的test_table表
登陆主机：
     '%'表示所有ip
     'localhost' 表示本机
     '192.168.10.2' 特定IP
//创建用户并分配给管理员权限
grant all  on dbname.* to username@'%' identified by '密码' WITH GRANT OPTION;

//查看用户权限
show grants for username;

//回收权限
revoke all on dbname.* from username@'%';
```