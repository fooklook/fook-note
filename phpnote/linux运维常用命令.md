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

- -a：指定输出文件名的后缀长度，默认为2个(aa,ab...)
- -d：指定输出文件名的后缀用数字代替
- -b：指定输出文件的最大字节数，如1k,1m...
- -C：指定每一个输出文件中单行的最大字节数
- -l：指定每一个输出文件的最大行数

```
split -6 README             #将README文件每六行分割成一个文件
split -l 100 log.log splog  #每个分块100行，不考虑大小。日志分析时应该有用。
```

paste：按行合并文件内容。

sort：对文件的文本内容排序。

uniq：去除重复行。

wc：统计文件的行数、单词数或字节数。

- -c或--bytes或--chars 只显示Bytes数。
- -l或--lines 只显示行数。
- -w或--words 只显示字数。

```
wc testfile           # testfile文件的统计信息  
3 92 598 testfile       # testfile文件的行数为3、单词数92、字节数598 
```

iconv：转换文件的编码格式。

dos2unix：将 DOS 格式文件转换成 UNIX 格式。

diff：全拼 difference，比较文件的差异，常用于文本文件。

vimdiff：命令行可视化文件比较工具，常用于文本文件。

```
vimdiff  FILE_LEFT  FILE_RIGHT
//或者
vim -d  FILE_LEFT  FILE_RIGHT
//或者
vim FILE_LEFT
:vertical diffsplit FILE_RIGHT
```

rev：反向输出文件内容。

grep/egrep：过滤字符串，三剑客老三。

join：按两个文件的相同字段合并。

tr：替换或删除字符。

- -c, --complement：反选设定字符。也就是符合 SET1 的部份不做处理，不符合的剩余部份才进行转换
- -d, --delete：删除指令字符
- -s, --squeeze-repeats：缩减连续重复的字符成指定的单个字符
- -t, --truncate-set1：削减 SET1 指定范围，使之与 SET2 设定长度相等

```
    \NNN 八进制值的字符 NNN (1 to 3 为八进制值的字符)
    \\ 反斜杠
    \a Ctrl-G 铃声
    \b Ctrl-H 退格符
    \f Ctrl-L 走行换页
    \n Ctrl-J 新行
    \r Ctrl-M 回车
    \t Ctrl-I tab键
    \v Ctrl-X 水平制表符
    CHAR1-CHAR2 ：字符范围从 CHAR1 到 CHAR2 的指定，范围的指定以 ASCII 码的次序为基础，只能由小到大，不能由大到小。
    [CHAR*] ：这是 SET2 专用的设定，功能是重复指定的字符到与 SET1 相同长度为止
    [CHAR*REPEAT] ：这也是 SET2 专用的设定，功能是重复指定的字符到设定的 REPEAT 次数为止(REPEAT 的数字采 8 进位制计算，以 0 为开始)
    [:alnum:] ：所有字母字符与数字
    [:alpha:] ：所有字母字符
    [:blank:] ：所有水平空格
    [:cntrl:] ：所有控制字符
    [:digit:] ：所有数字
    [:graph:] ：所有可打印的字符(不包含空格符)
    [:lower:] ：所有小写字母
    [:print:] ：所有可打印的字符(包含空格符)
    [:punct:] ：所有标点字符
    [:space:] ：所有水平与垂直空格符
    [:upper:] ：所有大写字母
    [:xdigit:] ：所有 16 进位制的数字
    [=CHAR=] ：所有符合指定的字符(等号里的 CHAR，代表你可自订的字符)

cat testfile |tr [:lower:] [:upper:] 
cat testfile |tr a-z A-Z    //将文件中小写字母转成大写
```

vi/vim：命令行文本编辑器。

命令模式

- i 切换到输入模式，以输入字符。
- x 删除当前光标所在处的字符。
- : 切换到底线命令模式，以在最底一行输入命令。

移动光标的方法

- h 或 向左箭头键(←) 	光标向左移动一个字符
- j 或 向下箭头键(↓) 	光标向下移动一个字符
- k 或 向上箭头键(↑) 	光标向上移动一个字符
- l 或 向右箭头键(→) 	光标向右移动一个字符
- [Ctrl] + [f] 	屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)
- [Ctrl] + [b] 	屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)
- [Ctrl] + [d] 	屏幕『向下』移动半页
- [Ctrl] + [u] 	屏幕『向上』移动半页
- +光标移动到非空格符的下一行
- -光标移动到非空格符的上一行
- n+空格 	那个 n 表示『数字』，例如 20 。按下数字后再按空格键，光标会向右移动这一行的n个字符。例如 20+空格 则光标会向后面移动20个字符距离。
- 0 或功能键[Home] 	这是数字『 0 』：移动到这一行的最前面字符处 (常用)
- $ 或功能键[End] 	移动到这一行的最后面字符处(常用)
- H 	光标移动到这个屏幕的最上方那一行的第一个字符
- M 	光标移动到这个屏幕的中央那一行的第一个字符
- L 	光标移动到这个屏幕的最下方那一行的第一个字符
- G 	移动到这个档案的最后一行(常用)
- nG 	n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20 行(可配合 :set nu)
- gg 	移动到这个档案的第一行，相当于 1G 啊！ (常用)
- n<Enter> 	n 为数字。光标向下移动 n 行(常用)

搜索替换

- /word 	向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！(常用) 
- ?word 	向光标之上寻找一个字符串名称为 word 的字符串。
- n 	这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！
- N 	这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。 例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird 。
- :n1,n2s/word1/word2/g 	n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为 word2 ！举例来说，在 100 到 200 行之间搜寻 vbird 并取代为 VBIRD 则：『:100,200s/vbird/VBIRD/g』。(常用)
- :1,$s/word1/word2/g 	从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！(常用)
- :1,$s/word1/word2/gc 	从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 word2 ！且在取代前显示提示字符给用户确认 (confirm) 是否需要取代！(常用)

删除、复制与贴上

- x, X 	在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) (常用)
- nx 	n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。
- dd 	删除游标所在的那一整行(常用)
- ndd 	n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 (常用)
- d1G 	删除光标所在到第一行的所有数据
- dG 	删除光标所在到最后一行的所有数据
- d$ 	删除游标所在处，到该行的最后一个字符
- d0 	那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符
- yy 	复制游标所在的那一行(常用)
- nyy 	n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用)
- y1G 	复制游标所在行到第一行的所有数据
- yG 	复制游标所在行到最后一行的所有数据
- y0 	复制光标所在的那个字符到该行行首的所有数据
- y$ 	复制光标所在的那个字符到该行行尾的所有数据
- p, P 	p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！ 举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20 行之后，亦即由 21 行开始贴。但如果是按下 P 呢？ 那么原本的第 20 行会被推到变成 30 行。 (常用)
- J 	将光标所在行与下一行的数据结合成同一行
- c 	重复删除多个数据，例如向下删除 10 行，[ 10cj ]
- u 	复原前一个动作。(常用)
- [Ctrl]+r 	重做上一个动作。(常用)
这个 u 与 [Ctrl]+r 是很常用的指令！一个是复原，另一个则是重做一次～ 利用这两个功能按键，你的编辑，嘿嘿！很快乐的啦！
- . 	不要怀疑！这就是小数点！意思是重复前一个动作的意思。 如果你想要重复删除、重复贴上等等动作，按下小数点『.』就好了！ (常用)

指令行的储存、离开等指令

- :w 	将编辑的数据写入硬盘档案中(常用)
- :w! 	若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！
- :q 	离开 vi (常用)
- :q! 	若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。
- :wq 	储存后离开，若为 :wq! 则为强制储存后离开 (常用)
- ZZ 	这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开！
- :w [filename] 	将编辑的数据储存成另一个档案（类似另存新档）
- :r [filename] 	在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面
- :n1,n2 w [filename] 	将 n1 到 n2 的内容储存成 filename 这个档案。
- :! command 	暂时离开 vi 到指令行模式下执行 command 的显示结果！例如
- 『:! ls /home』即可在 vi 当中察看 /home 底下以 ls 输出的档案信息！



三、文件压缩及解压缩命令（4 个）

tar：打包压缩。

- -c: 建立压缩档案
- -x：解压
- -t：查看内容
- -r：向压缩归档文件末尾追加文件
- -u：更新原压缩包中的文件

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

- -z：有gzip属性的
- -j：有bz2属性的
- -Z：有compress属性的
- -v：显示所有过程
- -O：将文件解开到标准输出

下面的参数-f是必须的

- -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

压缩

- tar –cvf jpg.tar *.jpg  将目录里所有jpg文件打包成tar.jpg
- tar –czf jpg.tar.gz *.jpg   将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
- tar –cjf jpg.tar.bz2 *.jpg 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
- tar –cZf jpg.tar.Z *.jpg   将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
- rar a jpg.rar *.jpg rar格式的压缩，需要先下载rar for linux
- zip jpg.zip *.jpg   zip格式的压缩，需要先下载zip for linux 

解压

- tar –xvf file.tar  解压 tar包
- tar -xzvf file.tar.gz 解压tar.gz
- tar -xjvf file.tar.bz2   解压 tar.bz2
- tar –xZvf file.tar.Z   解压tar.Z
- unrar e file.rar 解压rar
- unzip file.zip 解压zip


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

```
scp [可选参数] file_source file_target 
```

- -1： 强制scp命令使用协议ssh1
- -2： 强制scp命令使用协议ssh2
- -4： 强制scp命令只使用IPv4寻址
- -6： 强制scp命令只使用IPv6寻址
- -B： 使用批处理模式（传输过程中不询问传输口令或短语）
- -C： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
- -p：保留原文件的修改时间，访问时间和访问权限。
- -q： 不显示传输进度条。
- -r： 递归复制整个目录。
- -v：详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
- -c cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
- -F ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh。
- -i identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
- -l limit： 限定用户所能使用的带宽，以Kbit/s为单位。
- -o ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式，
- -P port：注意是大写的P, port是指定数据传输用到的端口号
- -S program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。

从本地复制到远程 

```
scp local_file remote_username@remote_ip:remote_folder 
或者 
scp local_file remote_username@remote_ip:remote_file 
或者 
scp local_file remote_ip:remote_folder 
或者 
scp local_file remote_ip:remote_file 
```

从远程复制到本地 

```
scp root@www.runoob.com:/home/root/others/music /home/space/music/1.mp3 
scp -r www.runoob.com:/home/root/others/ /home/space/music/
```

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