##ubuntu防火墙设置

###安装

```shell
sudo apt-get install ufw
```
###启用

```shell
//开启防火墙，并在系统启动时自动开启
sudo ufw enable 
//关闭所有外部对本机的访问，但本机访问外部正常。
sudo ufw default deny 
```

###开启/禁用

```shell
sudo ufw allow|deny [service] 
```

打开或关闭某个端口，例如:  
sudo ufw allow smtp　允许所有的外部IP访问本机的25/tcp (smtp)端口  
sudo ufw allow 22/tcp 允许所有的外部IP访问本机的22/tcp (ssh)端口   
sudo ufw allow 53 允许外部访问53端口(tcp/udp)   
sudo ufw allow from 192.168.1.100 允许此IP访问所有的本机端口   
sudo ufw allow proto udp 192.168.0.1 port 53 to 192.168.0.2 port 53   
sudo ufw deny smtp 禁止外部访问smtp服务  
sudo ufw delete allow smtp 删除上面建立的某条规则  

###查看防火墙状态 

```shell
sudo ufw status
```