##搭建Git服务器

###安装git

```shell
sudo apt-get install git
```

###创建一个git用户，用来运行git服务

```shell
sudo adduser git
//根据提示输入密码
//根据提示输入对应的用户信息，不填也无所谓。
```

###为git用户添加sudo命令的权限

sudo是提权命令，但不是所有用户都有提权的权限，所以需要进行相应的设置。

```shell
//切换到root用户下
su root%
//添加sudo文件的写权限
sudo chmod u+w /etc/sudoers
//编辑sudoers文件
vi /etc/sudoers
//在文本的后面添加一下的任意一行
//git            ALL=(ALL)                ALL
//%git           ALL=(ALL)                ALL
//git            ALL=(ALL)                NOPASSWD: ALL
//%git          ALL=(ALL)                NOPASSWD: ALL
//撤销sudoers文件写权限
chmod u-w /etc/sudoers
```

以下为对应列的权限关系

1. 允许用户git执行sudo命令(需要输入密码).
2. 允许用户组git里面的用户执行sudo命令(需要输入密码).
3. 允许用户git执行sudo命令,并且在执行的时候不输入密码.
4. 允许用户组git里面的用户执行sudo命令,并且在执行的时候不输入密码.

现在git用户就可以使用sudo命令了。

###将登录用户的公钥添加都服务器上

首先客户端需要创建ssh密钥，不清楚的可以参考《[ssh公钥自动登录服务器](./ssh公钥自动登录服务器.md)》创建ssh密钥。

####手动添加

创建/home/git/.ssh/authorized_keys文件，将创建的公钥，即id_rsa.pub文件中的内容copy到该文件中，可添加多个客户端用户，一行添加一个。

####命令上传

在客户端终端执行以下命令

```shell
cat ~/.ssh/id_rsa.pub | ssh git@serverip "cat ->> ~/.ssh/authorized_keys"
//根据需要输入git用户密码
```

###创建Git仓库

在服务器端，进入/srv目录，并创建Git仓库。

```shell
cd /srv
//创建test仓库
sudo git init --bare test.git
//修改test.git权限
sudo chown -R git:git sample.git
```

###禁止git用户通过shell登录

```shell
//切换到root用户
su root
//编辑/etc/passwd文件
//找到git用户权限
//将 git:x:1001:1001:,,,:/home/git:/bin/bash
//改为 git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

###本地clone远程仓库

现在，可以通过git clone命令克隆远程仓库了。

```shell
git clone git@server:/srv/sample.git
```