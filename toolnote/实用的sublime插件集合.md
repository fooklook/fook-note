##实用的sublime插件集合

###安装包管理

要添加、删除、禁用和查找sublime插件，则需要先安装Package Control（安装包管理器）。

####安装方法

1. CTRL+` ，出现控制台
2. 粘贴以下代码至控制台
3. 在控制台运行以下代码

sublime Text2

```code
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

sublime Text3

```code
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

等安装后，重启sublime text。如果在Perferences->package settings中看到package control这一项，则安装成功。

按下Ctrl+Shift+P调出命令面板

输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。

###相关插件介绍

####Emmet

快速补全html代码。

常用代码配合tab键，用来完成补充。

```html
html:5
ul>li#idName.className[data-name=123]*5
```

####JSFormat

Javascript的代码格式化插件。

很多网站的JS代码都进行了压缩，一行式的甚至混淆压缩，这让我们看起来很吃力。而这个插件能帮我们把原始代码进行格式的整理，包括换行和缩进等等，是代码一目了然，更快读懂~

快捷键为Ctrl+Alt+F

####LESS

LESS高亮插件

sublime没有支持less的语法高亮，所以这个插件可以帮上我们

####sublime-autoprefixer

CSS添加私有前缀

Ctrl+Shift+P，选择autoprefixer即可。需要安装node.js。

####Bracket Highlighter

代码匹配

可匹配[], (), {}, “”, ”, <tag></tag>，高亮标记，便于查看起始和结束标记

####jQuery

jQ函数提示

快捷输入jQ函数，是偷懒的好方法

####Doc​Blockr

生成优美注释

####Color​Picker

调色板

需要输入颜色时，可直接选取颜色，快捷键Windows: ctrl+shift+c。

####ConvertToUTF8

文件转码成utf-8

通过本插件，您可以编辑并保存目前编码不被 Sublime Text 支持的文件，特别是中日韩用户使用的 GB2312，GBK，BIG5，EUC-KR，EUC-JP ，ANSI等。ConvertToUTF8 同时支持 Sublime Text 2 和 3。安装插件后自动转换为utf-8格式。

####AutoFileName

快捷输入文件名

自动完成文件名的输入，如图片选取。输入”/”即可看到相对于本项目文件夹的其他文件。