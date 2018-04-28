## linux运维常用命令

man： 查看命令帮助，命令的词典，更复杂的还有 info，但不常用。

help：查看 Linux 内置命令的帮助，比如 cd 命令。

### 文件和目录操作命令 (18 个)

ls：全拼 list，功能是列出目录的内容及其内容属性信息。

cd：全拼 change directory，功能是从当前工作目录切换到指定的工作目录。

cp：全拼 copy，其功能为复制文件或目录。

find：查找的意思，用于查找目录及目录下的文件。

```
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录
find / -user user1 搜索属于用户 'user1' 的文件和目录
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备
```

mkdir：全拼 make directories，其功能是创建目录。

mv：全拼 move，其功能是移动或重命名文件。

pwd：全拼 print working directory，其功能是显示当前工作目录的绝对路径。

rename：用于重命名文件。

rm：全拼 remove，其功能是删除一个或多个文件或目录。

rmdir：全拼 remove empty directories，功能是删除空目录。

touch：创建新的空文件，改变已有文件的时间戳属性。

tree：功能是以树形结构显示目录下的内容。

basename：显示文件名或目录名。

dirname：显示文件或目录路径。

chattr：改变文件的扩展属性。

lsattr：查看文件扩展属性。

file：显示文件的类型。

md5sum：计算和校验文件的 MD5 值。

### 查看文件及内容处理命令（21 个）

cat：全拼 concatenate，功能是用于连接多个文件并且打印到屏幕输出或重定向到指定文件中。

- -n 或 –number 由 1 开始对所有输出的行数编号
- -b 或 –number-nonblank 和 -n 相似，只不过对于空白行不编号
- -s 或 –squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
- -v 或 –show-nonprinting

```
cat   filename                                  //一次显示整个文件
cat  >  filename                                //从键盘创建一个文件
cat   file1   file2  > file                     //将几个文件合并为一个文件
cat -n linuxfile1 > linuxfile2                  //把 linuxfile1 的档案内容加上行号后输入 linuxfile2 这个档案里
cat -b linuxfile1 linuxfile2 >> linuxfile3      //把 linuxfile1 和 linuxfile2 的档案内容加上行号(空白行不加)之后将内容附加到linuxfile3 里。
```

tac：tac 是 cat 的反向拼写，因此命令的功能为反向显示文件内容。

more：分页显示文件内容。

- -num 一次显示的行数
- -d 提示使用者，在画面下方显示 [Press space to continue, 'q' to quit.] ，如果使用者按错键，则会显示 [Press 'h' for instructions.] 而不是 '哔' 声
- -l 取消遇见特殊字元 ^L（送纸字元）时会暂停的功能
- -f 计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
- -p 不以卷动的方式显示每一页，而是先清除萤幕后再显示内容
- -c 跟 -p 相似，不同的是先显示内容再清除其他旧资料
- -s 当遇到有连续两行以上的空白行，就代换为一行的空白行
- -u 不显示下引号 （根据环境变数 TERM 指定的 terminal 而有所不同）
- +/pattern 在每个文档显示前搜寻该字串（pattern），然后从该字串之后开始显示
- +num 从第 num 行开始显示
- fileNames 欲显示内容的文档，可为复数个数

常用操作命令

- Enter 向下n行，需要定义。默认为1行
- Ctrl+F 向下滚动一屏
- 空格键 向下滚动一屏
- Ctrl+B 返回上一屏
- = 输出当前行的行号
- :f 输出文件名和当前行的行号
- V 调用vi编辑器
- !命令 调用Shell，并执行命令
- q 退出more

```
more -s testfile                        //逐页显示 testfile 文档内容，如有连续两行以上空白行则以一行空白行显示。
more +20 testfile                       //从第 20 行开始显示 testfile 之文档内容
```

less：分页显示文件内容，more 命令的相反用法，可以上下翻页。

head：显示文件内容的头部。

tail：显示文件内容的尾部。

- -f 该参数用于监视File文件增长。
- -c Number 从 Number 字节位置读取指定文件
- -n Number 从 Number 行位置读取指定文件。
- -m Number 从 Number 多字节字符位置读取指定文件，比方你的文件假设包括中文字，假设指定-c参数，可能导致截断，但使用-m则会避免该问题。
- -b Number 从 Number 表示的512字节块位置读取指定文件。
- -k Number 从 Number 表示的1KB块位置读取指定文件。

```
tail -f filename            //监视filename文件的尾部内容（默认10行，相当于增加参数 -n 10），刷新显示在屏幕上。退出，按下CTRL+C。
tail -n 20 filename      //显示filename最后20行。
tail -r -n 10 filename   //逆序显示filename最后10行。 
```

cut：将文件的每一行按指定分隔符分割并输出。

- -b：仅显示行中指定直接范围的内容；
- -c：仅显示行中指定范围的字符；
- -d：指定字段的分隔符，默认的字段分隔符为“TAB”；
- -f：显示指定字段的内容；
- -n：与“-b”选项连用，不分割多字节字符；
- --complement：补足被选择的字节、字符或字段；
- --out-delimiter=<字段分隔符>：指定输出内容是的字段分割符；
- --help：显示指令的帮助信息；
- --version：显示指令的版本信息。

```
//例如有一个学生报表信息，包含No、Name、Mark、Percent：
cat test.txt
//输出
No Name Mark Percent
01 tom 69 91
02 jack 71 87
03 alex 68 98
cut -f 1 test.txt
//输出
No
01
02
03
cut -f 2,3 test.txt
//输出
Name Mark
tom 69
jack 71
alex 68
cut -f2 --complement test.txt
//输出
No Mark Percent
01 69 91
02 71 87
03 68 98
cat test2.txt
//使用 -d 选项指定字段分隔符：
No;Name;Mark;Percent
01;tom;69;91
02;jack;71;87
03;alex;68;98

cut -f2 -d";" test2.txt
//输出
Name
tom
jack
alex
```

split：分割文件为不同的小片段。

paste：按行合并文件内容。

sort：对文件的文本内容排序。

uniq：去除重复行。

wc：统计文件的行数、单词数或字节数。

iconv：转换文件的编码格式。

dos2unix：将 DOS 格式文件转换成 UNIX 格式。

diff：全拼 difference，比较文件的差异，常用于文本文件。

vimdiff：命令行可视化文件比较工具，常用于文本文件。

rev：反向输出文件内容。

grep/egrep：过滤字符串，三剑客老三。

join：按两个文件的相同字段合并。

tr：替换或删除字符。

vi/vim：命令行文本编辑器。

```

```

三、文件压缩及解压缩命令（4 个）

tar：打包压缩。oldboy

unzip：解压文件。

gzip：gzip 压缩工具。

zip：压缩工具。

四、信息显示命令（11 个）

uname -a：显示操作系统相关信息的命令。

hostname：显示或者设置当前系统的主机名。

dmesg：显示开机信息，用于诊断系统故障。

uptime：显示系统运行时间及负载。

stat：显示文件或文件系统的状态。

du：计算磁盘空间使用情况。

df：报告文件系统磁盘空间的使用情况。

top：实时显示系统资源使用情况。

free：查看系统内存。

date：显示与设置系统时间。

cal：查看日历等时间信息。

五、搜索文件命令（2 个）

which：查找二进制命令，按环境变量 PATH 路径查找。

find：从磁盘遍历查找文件或目录。

六、用户管理命令（10 个）

useradd：添加用户。

usermod：修改系统已经存在的用户属性。

userdel：删除用户。

groupadd：添加用户组。

passwd：修改用户密码。

chage：修改用户密码有效期限。

id：查看用户的 uid,gid 及归属的用户组。

su：切换用户身份。

visudo：编辑 / etc/sudoers 文件的专属命令。

sudo：以另外一个用户身份（默认 root 用户）执行事先在 sudoers 文件允许的命令。

七、基础网络操作命令（11 个）

telnet：（23）使用 TELNET 协议远程登录。

ssh：（22）使用 SSH 加密协议远程登录。

scp：全拼 secure copy，用于不同主机之间复制文件。

wget：命令行下载文件。

ping：测试主机之间网络的连通性。

route：显示和设置 linux 系统的路由表。

ifconfig：查看、配置、启用或禁用网络接口的命令。

ifup：启动网卡。

ifdown：关闭网卡。

netstat：查看网络状态。

```
netstat -tnlp | grep 8080       //查看端口占用情况
```

ss：查看网络状态。

八、深入网络操作命令（9 个）

nmap：网络扫描命令。

lsof：全名 list open files，也就是列举系统中已经被打开的文件。

mail：发送和接收邮件。

mutt：邮件管理命令。

nslookup：交互式查询互联网 DNS 服务器的命令。

dig：查找 DNS 解析过程。

host：查询 DNS 的命令。

traceroute：追踪数据传输路由状况。

tcpdump：命令行的抓包工具。

九、有关磁盘与文件系统的命令（16 个）

mount：挂载文件系统。

```
mount /dev/hda2 /mnt/hda2 挂载一个叫做hda2的盘 - 确定目录 '/ mnt/hda2' 已经存在
mount /dev/fd0 /mnt/floppy 挂载一个软盘
mount /dev/cdrom /mnt/cdrom 挂载一个cdrom或dvdrom
mount /dev/hdc /mnt/cdrecorder 挂载一个cdrw或dvdrom
mount /dev/hdb /mnt/cdrecorder 挂载一个cdrw或dvdrom
mount -o loop file.iso /mnt/cdrom 挂载一个文件或ISO镜像文件
mount -t vfat /dev/hda5 /mnt/hda5 挂载一个Windows FAT32文件系统
mount /dev/sda1 /mnt/usbdisk 挂载一个usb 捷盘或闪存设备
mount -t smbfs -o username=user,password=pass //WinClient/share /mnt/share 挂载一个windows网络共享
```

umount：卸载文件系统。

```
umount /dev/hda2 卸载一个叫做hda2的盘 - 先从挂载点 '/ mnt/hda2' 退出
umount -n /mnt/hda2 运行卸载操作而不写入 /etc/mtab 文件- 当文件为只读或当磁盘写满时非常有用

//无法正常卸载磁盘
fuser -m -v /dev/sdb1 运行下面命令看一下哪个用户哪个进程占用着此设备
fuser -m -v -k /dev/sdb1 运行下面命令杀掉占用此设备的进程
//或者
fuser -m -v -k -i  /dev/sdb1 (每杀掉一下进程会让你确认）
//再umount
```

fsck：检查并修复 Linux 文件系统。

dd：转换或复制文件。

dumpe2fs：导出 ext2/ext3/ext4 文件系统信息。

dump：ext2/3/4 文件系统备份工具。

fdisk：磁盘分区命令，适用于 2TB 以下磁盘分区。

parted：磁盘分区命令，没有磁盘大小限制，常用于 2TB 以下磁盘分区。

mkfs：格式化创建 Linux 文件系统。

partprobe：更新内核的硬盘分区表信息。

e2fsck：检查 ext2/ext3/ext4 类型文件系统。

mkswap：创建 Linux 交换分区。

swapon：启用交换分区。

swapoff：关闭交换分区。

sync：将内存缓冲区内的数据写入磁盘。

resize2fs：调整 ext2/ext3/ext4 文件系统大小。

系统权限及用户授权相关命令（4 个）

chmod：改变文件或目录权限。

- u表示该文件的拥有者，g表示与该文件的拥有者属于同一个群体(group)者，o表示其他以外的人，a表示这三者皆是。
- +表示增加权限、-表示取消权限、=表示唯一设定权限。
- r表示可读取，w表示可写入，x表示可执行，X表示只有当该文件是个子目录或者该文件已经被设定过为可执行。
- -c : 若该文件权限确实已经更改，才显示其更改动作
- -f : 若该文件权限无法被更改也不要显示错误讯息
- -v : 显示权限变更的详细资料
- -R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更)

```
chmod ugo+r file1.txt                   //将文件 file1.txt 设为所有人皆可读取 
chmod a+r file1.txt                     //将文件 file1.txt 设为所有人皆可读取
chmod ug+w,o-w file1.txt file2.txt      //将文件 file1.txt 与 file2.txt 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入
chmod u+x ex1.py                        //将 ex1.py 设定为只有该文件拥有者可以执行
chmod -R a+r *                          //将目前目录下的所有文件与子目录皆设为任何人可读取
sudo chmod 777 -R xxx （更改文件夹及其子文件夹权限为777）
sudo chmod 600 ××× （只有所有者有读和写的权限）
sudo chmod 644 ××× （所有者有读和写的权限，组用户只有读的权限）
sudo chmod 700 ××× （只有所有者有读和写以及执行的权限）
sudo chmod 666 ××× （每个人都有读和写的权限）
sudo chmod 777 ××× （每个人都有读和写以及执行的权限
```

chown：改变文件或目录的属主和属组。

```
chown zhangy:zhangy nginx.conf         #将nginx.conf所属用户和组改为zhangy,zhangy
chown -R zhangy:zhangy www            #将www目录，所属用户和组改为zhangy,zhangy
chown root nginx.conf        #将nginx.conf，所属用户改为root
```

chgrp：更改文件用户组。

umask：显示或设置权限掩码。

十、查看系统用户登陆信息的命令（7 个）

whoami：显示当前有效的用户名称，相当于执行 id -un 命令。

who：显示目前登录系统的用户信息。

w：显示已经登陆系统的用户列表，并显示用户正在执行的指令。

last：显示登入系统的用户。

lastlog：显示系统中所有用户最近一次登录信息。

users：显示当前登录系统的所有用户的用户列表。

finger：查找并显示用户信息。

十一、内置命令及其它（19 个）

echo：打印变量，或直接输出指定的字符串

printf：将结果格式化输出到标准输出。

rpm：管理 rpm 包的命令。

yum：自动化简单化地管理 rpm 包的命令。

watch：周期性的执行给定的命令，并将命令的输出以全屏方式显示。

alias：设置系统别名。

unalias：取消系统别名。

date：查看或设置系统时间。

clear：清除屏幕，简称清屏。

history：查看命令执行的历史纪录。

eject：弹出光驱。

time：计算命令执行时间。

nc：功能强大的网络工具。

xargs：将标准输入转换成命令行参数。

exec：调用并执行指令的命令。

export：设置或者显示环境变量。

unset：删除变量或函数。

type：用于判断另外一个命令是否是内置命令。

bc：命令行科学计算器

十二、系统管理与性能监视命令 (9 个)

chkconfig：管理 Linux 系统开机启动项。

vmstat：虚拟内存统计。

mpstat：显示各个可用 CPU 的状态统计。

iostat：统计系统 IO。

sar：全面地获取系统的 CPU、运行队列、磁盘 I/O、分页（交换区）、内存、 CPU 中断和网络等性能数据。

ipcs：用于报告 Linux 中进程间通信设施的状态，显示的信息包括消息列表、共享内存和信号量的信息。

ipcrm：用来删除一个或更多的消息队列、信号量集或者共享内存标识。

strace：用于诊断、调试 Linux 用户空间跟踪器。我们用它来监控用户空间进程和内核的交互，比如系统调用、信号传递、进程状态变更等。

ltrace：命令会跟踪进程的库函数调用, 它会显现出哪个库函数被调用。

十三、关机 / 重启 / 注销和查看系统信息的命令（6 个）

shutdown：关机。

halt：关机。

poweroff：关闭电源。

logout：退出当前登录的 Shell。

exit：退出当前登录的 Shell。

Ctrl+d：退出当前登录的 Shell 的快捷键。

进程管理相关命令（15 个）

bg：将一个在后台暂停的命令，变成继续执行 （在后台执行）。

fg：将后台中的命令调至前台继续运行。

jobs：查看当前有多少在后台运行的命令。

kill：终止进程。

killall：通过进程名终止进程。

pkill：通过进程名终止进程。

crontab：定时任务命令。

ps：显示进程的快照。

pstree：树形显示进程。

nice/renice：调整程序运行的优先级。

nohup：忽略挂起信号运行指定的命令。

pgrep：查找匹配条件的进程。

runlevel：查看系统当前运行级别。

init：切换运行级别。

service：启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态。