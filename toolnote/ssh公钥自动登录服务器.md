##ssh公钥自动登录服务器
第一步，在服务器上安装openssh-server，用来实现远程登录。

```shell
sudo apt-get install openssh-server
```

第二步，服务器安装ssh服务

```shell
sudo apt-get install ssh
//验证是否安装成功
ssh -v
```

第三步,在本地创建公钥密钥对

```shell
//创建ssh公钥私钥对，通过rsa加密
ssh-keygen -t rsa
//根据提示输入--》储存文件的位置　　　　/Users/root/.ssh/id_rsa 这是默认地址
//　　　　　　　--》密码
//  　　　　　　--》再次输入密码
//将.ssh文件权限设为700
sudo chmod 700 .ssh
//将id_rsa文件权限设为600
sudo chmod 600 id_rsa
```

第四步，将公钥上传到服务器

```shell
cat ~/.ssh/id_rsa.pub | ssh root@远程服务器ip 'cat - >> ~/.ssh/authorized_keys'
//然后输入登录密码
```

如果上传不成功，可以手动创建anthorized_keys文件，并且将id_rsa.pub文件的内容粘贴在里面。

并且将anthorized_keys文件权限设为600。

然后通过命令 `ssh root@服务器IP地址` 登录服务器。

如果依然提示需要输入密码：

可以执行一下命令加载私钥文件

```shell
//整理ssh代理
ssh-agent bash
//加载私钥
ssh-add ~/.ssh/id_rsa
```