##mysql数据库操作
###快速备份数据库
通常备份数据库都是借用相关数据库管理软件，但是当数据量较大时，数据库管理软件这种通过对单个表逐个进行备份的效率太慢。建议使用命令进行备份，相对来讲，比管理软件进行备份的效率更高。

```shell
//这是一个shell命令，不是mysql命令。
mysqldump dbname -u username -ppassword --add-drop-table > filename.sql
```
###拷贝数据库
```shell
//本地数据库
mysqldump dbname -u username -ppassword --add-drop-table | mysql dbname1 -u username -ppassword
//非本地数据库
mysqldump dbname -u username -ppassword --add-drop-table | mysql dbname -h ip -u username -ppassword
```