##laravel在windows平台安装教程
###前提说明
laravel框架和以往的框架（如：yii1.1、CI框架、TP框架），有很大的不同。  
以往的框架是直接通过下载，然后将框架代码，放在网站的根目录中，然后通过浏览器就可以访问。一般情况下，还会有一个简单的demo，让用户确认已经安装成功。  
但laravel框架一般情况下是通过composer管理工具实现安装的，就像通过命令行安装软件一样。  
在这里要提示从以往的框架中，学习laravel的同学，首先要摒弃掉手工管理代码的习惯。在laravel中，所有的文件都是可以通过命令行进行安装，而不需要进行手动创建。希望大家能养成这样的习惯。
###安装composer
首先进入官网下载安装包**[点击下载composer安装包](https://getcomposer.org/Composer-Setup.exe)**。  
composer安装过程需要开启php的openssl扩展。
然而如果时安装wamp的同学，通过wamp控制台打开扩展后，安装过程中可能依然提示需要开启openssl扩展。  
那是因为wamp安装后，会有两个php.ini文件，分别是wamp在apache和在CLI（命令行）模式下使用不同的配置文件，所以你需要找到C:\wamp\bin\php\php5.3.3\目录下的php.ini文件，开启openssl扩展。  
接下来你就可以顺利安装composer了。
###安装laravel
虽然官网上提供了，可以实现下载laravel安装包，然后通过laravel new project的方式安装laravel。但因为国内防火墙的原因，下载起来很慢，而且在windows平台，命令也无法正常执行。所以建议直接通过  

    composer create-project laravel/laravel --prefer-dist

命令，进行安装。
在安装的过程中，可能需要你输入token,这是什么鬼，不管。
要想获得这个token，你首先要注册一个github账号。  
点击头像-->settings-->Personal access tokens，在这里面生成一个token就可以了。  
安装过程很慢，请耐心等待。
安装成功后，你就会发现在你安装的目录下，多了一个laravel的文件夹。  
建议学习过程查看[laravel5.0文档](http://www.golaravel.com/laravel/docs/5.0/)  
larvel视频教程网站[Laracasts](https://laracasts.com),国外网站加载比较慢。

