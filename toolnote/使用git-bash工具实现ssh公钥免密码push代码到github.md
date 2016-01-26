##使用git-bash工具实现ssh公钥免密码push代码到github
>环境：windows系统+[git-bash工具](../toolnote/git-bash工具使用.md)

###创建本地的密钥
因为git-bash本身集成了ssh，所以我们可以方便的直接使用ssh的相关命令。  

```linux
ssh-keygen -t rsa  //创建ssh密钥
```
根据提示输入

- 存储文件密钥的位置（直接回车键，创建在默认的目录下）
- 输入密码
- 再次输入密码（如果设置了密码，每次提交代码需要输入这个密码）

###将公钥添加到github
点击右端小头像-->settings-->SSH keys
通过命令打开公钥

```vim
vim ~/.ssh/id_rsa.pub
```
将公钥复制到github上。  

###使用中可能遇到的问题
####找不到ssh私钥
通过ssh-add ~/.ssh/id_rsa命令，添加私钥。

####无法执行添加私钥
在添加私钥时，经常会出现

```shell
ould not open a connection to your authentication agent.
```
很容易解决，执行一下这个命令就可以。

```shell
ssh-agent bash
```