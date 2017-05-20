##国内快速安装composer

国内通过setup安装composer的速度越来越慢，但在windows环境下，安装composer并不是只能通过setup的方式安装。

###下载composer.phar文件

[下载地址](https://getcomposer.org/download/)

在Manual Download下载列表中，选择下载版本。

###放置composer.phar文件

将composer.phar文件放置到php.exe的同级目录下。

查看PATH是否设置了php的全局环境变量。如果没有，需要配置一下。

###创建composer.bat文件

在php.exe和composer.phar的同级目录下，创建composer.bat文件。编辑内容为：

```
@php "%~dp0composer.phar" %*
```

现在就可以在cmd环境中，执行composer命令了。

但是在git-bash环境中，并不能执行。

###创建composer文件

同样在同级环境中，创建composer文件。编辑内容为：

```
#!/usr/bin/env sh

# php /path/to/composer.phar $*
php `dirname $0`/composer.phar $*
```

重新打开git-bash，就可以使用composer命令了。