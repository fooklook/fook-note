## gitnote
### linux下安装git
```shell
sudo apt-get install git
```
### windows下使用git
建议安装git bash [git-bash工具使用](../toolnote/git-bash工具使用.md)

### 设置机器标识

git config --global user.name "Your Name"

git config --global user.email "email@example.com"

### 创建仓库

创建一个文件夹，并将文件夹变成版本仓库

mkdir folder

cd folder

git init

### 提交文件到仓库

创建README.md文件

将文件添加到仓库

git add README.md

提交文件到仓库

git commit -m "message"

git commit --amend 可以对上一次的提交做修改

### 查看版本区别

查看版本情况

git status

对比当前修改和仓库版本的不同

git diff filename

### 版本后退

查看版本日志

git log

回滚到上个版本

git reset --hard HRAD^

git reset --hard 版本号

单个文件回滚

```
git log filename                //查看文件版本日志

git reset logname filename      //回滚

git commit -m "reset reason"    //日志

git checkout filename           //更新到工作目录
```

查看历史命令

git reflog

通过查看历史命令获取版本号

git rm filename

git checkout -- filename

git reset HEAD filename

版本回退后，强制推送到远程分支

git push -f

### 修改远程服务器链接等问题
添加远程服务器连接

git remote add origin url

修改远程服务器链接

git remote set-url origin url

查看远程仓库地址

git remote -v

### 分支管理

查看分支

git branch

创建分支并切换到该分支

git checkout -b dev

创建分支

git branch dev

切换到该分支

git checkout dev

删除该分支

git branch -d dev

合并某分支到当前分支

git merge dev

删除远程分支

git push origin --delete dev

### 标签管理

给当前commit打标签

git tag tagname

给指定的commit打标签

git tag tagname commitnum

创建带有说明的标签

git tag -a tagname -m "version 0.1 released" commitnum

查询标签

git tag

查询标签修改内容

git show tagname

删除标签

git tag -d tagname

在本地打上标签，只会在本地，如果想要推送某个标签到远程

git push origin v1.0

一次性推送全部尚未推送到远程的本地标签

git push origin --tags

删除远程的标签，先删除本地标签，然后在执行以下代码

git push origin :refs/tags/tagname