##在windows下安装laravel
###前提说明
laravel框架和以往的框架（如：yii1.1、CI框架、TP框架），有很大的不同。  
以往的框架是直接通过下载，然后将框架代码，放在网站的根目录中，然后通过浏览器就可以访问。一般情况下，还会有一个简单的demo，让用户确认已经安装成功。  
但laravel框架一般情况下是通过composer管理工具实现安装的，就像通过命令行安装软件一样。  
在这里要提示从以往的框架中，学习laravel的同学，首先要摒弃掉手工管理代码的习惯。在laravel中，所有的文件都是可以通过命令行进行安装，而不需要进行手动创建。希望大家能养成这样的习惯。
###安装条件
首先，我们这里是基于laravel5.0以上版本的，其它版本不做介绍和区别。

- php5.4以上版本(这里只需要这个条件)

###安装composer
首先进入官网下载安装包[点击下载composer安装包](https://getcomposer.org/Composer-Setup.exe)。

composer安装过程需要开启php的openssl扩展。

然而如果时安装wamp的同学，通过wamp控制台打开扩展后，安装过程中可能依然提示需要开启openssl扩展。

那是因为wamp安装后，会有两个php.ini文件，分别是wamp在apache和在CLI（命令行）模式下使用不同的配置文件，所以你需要找到C:\wamp\bin\php\php5.4.12\目录下的php.ini文件，开启openssl扩展。

接下来你就可以顺利安装composer了。

###安装laravel
虽然官网上提供了，可以实现下载laravel安装包，然后通过laravel new project的方式安装laravel。但因为国内防火墙的原因，下载起来很慢，而且在windows平台，命令也无法正常执行。所以建议直接通过  

```shell
    composer create-project laravel/laravel projectname --prefer-dist
```
命令，进行安装。

>这里不推荐使用官方的通过laravel命令安装。因为通过laravel命令安装，安装的版本是laravel5.1，需要php5.6以上版本支持。所以建议通过该方式安装。


在安装的过程中，可能需要你输入token,这是什么鬼，不管。
要想获得这个token，你首先要注册一个github账号。  
点击头像-->settings-->Personal access tokens，在这里面生成一个token就可以了。  
安装过程很慢，请耐心等待。
安装成功后，你就会发现在你安装的目录下，多了一个projectname(自己根据项目需求命名)的文件夹。  
建议学习过程查看[laravel5.0文档](http://www.golaravel.com/laravel/docs/5.0/)  
larvel视频教程网站[Laracasts](https://laracasts.com),国外网站加载比较慢。
###更改Composer源
上面说到，国外下载很慢，那么这里就通过修改Composer源，就可以飞一样的数据进行安装。

通过

```shell
composer config -l -g
```
命令，查看[home]中，composer的安装配置目录。
进入该目录后，编辑config.json文件（没有的话，自己手动创建）为:

```json
    {
        "config": {},
    	"repositories": [{
            "type": "composer",
            "url": "http://comproxy.cn/repo/packagist"
        }, {
            "packagist": false
        }]
    }
```
再通过命令安装，尝试一下，飞一样的速度。