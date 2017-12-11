## linux命令

### linux定时任务 crontab

ubuntu下默认安装并且开机启动了crontab

所以我们要添加定时任务只需要添加相应的规则

- crontab -e
- a进入编写模式
- ctrl+c退出编写模式
- shift+qw退出编辑器模式
- wq!保存并退出

在打开的文件中,添加规则

```
m          h              dom                             mon                         dow   command
m:      0～59  表示分
h:      1～23  表示小时
dom: 1～31  表示日
mon: 1～12  表示月份
dow:  0～6    表示星期（其中0表示星期日）
eg:
*/1    *    *    *    * echo "hello word"    >>/var/www/html/error.txt                //每隔一分钟，执行一次,结果输出到error.txt文件
```

### grep命令

Linux系统中grep命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹 配的行打印出来。

#### 参数：

- -I ：忽略大小写
- -c ：打印匹配的行数
- -l ：从多个文件中查找包含匹配项
- -v ：查找不包含匹配项的行
- -n：打印包含匹配项的行和行标

#### RE（正则表达式）

- \ 忽略正则表达式中特殊字符的原有含义
- ^ 匹配正则表达式的开始行
- $ 匹配正则表达式的结束行
- \\\< 从匹配正则表达式的行开始
- \\\> 到匹配正则表达式的行结束
- [ ] 单个字符；如[A] 即A符合要求
- [ - ] 范围 ；如[A-Z]即A，B，C一直到Z都符合要求
- . 所有的单个字符
- \* 所有字符，长度可以为0 

#### 举例

- cat size.txt | grep [a-b]
- php -i | grep TIME -cI

### apt-get

- apt-cache search package 搜索包
- apt-cache show package 获取包的相关信息，如说明、大小、版本等
- sudo apt-get install package 安装包
- sudo apt-get install package - - reinstall 重新安装包
- sudo apt-get -f install 修复安装"-f = ——fix-missing"
- sudo apt-get remove package 删除包
- sudo apt-get remove package - - purge 删除包，包括删除配置文件等
- sudo apt-get update 更新源
- sudo apt-get upgrade 更新已安装的包
- sudo apt-get dist-upgrade 升级系统
- sudo apt-get dselect-upgrade 使用 dselect 升级
- apt-cache depends package 了解使用依赖
- apt-cache rdepends package 是查看该包被哪些包依赖
- sudo apt-get build-dep package 安装相关的编译环境
- apt-get source package 下载该包的源代码
- sudo apt-get clean && sudo apt-get autoclean 清理无用的包
- sudo apt-get check 检查是否有损坏的依赖