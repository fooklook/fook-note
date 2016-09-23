##gitnote
###linux下安装git
```shell
sudo apt-get install git
```
###windows下使用git
建议安装git bash [git-bash工具使用](../toolnote/git-bash工具使用.md)

###设置机器标识

git config --global user.name "Your Name"

git config --global user.email "email@example.com"

###创建仓库

创建一个文件夹，并将文件夹变成版本仓库

mkdir folder

cd folder

git init

###提交文件到仓库

创建README.md文件

将文件添加到仓库

git add README.md

提交文件到仓库

git commit -m "message"

###查看版本区别

查看版本情况

git status

对比当前修改和仓库版本的不同

git diff filename

###版本后退

查看版本日志

git log

回滚到上个版本

git reset --hard HRAD^

git reset --hard 版本号

查看历史命令

git reflog

通过查看历史命令获取版本号

git rm filename

git checkout -- filename

git reset HEAD filename

###修改远程服务器链接等问题
添加远程服务器连接

git remote add origin1 url

修改远程服务器链接

git remote set-url origin url

查看远程仓库地址

git remote -v