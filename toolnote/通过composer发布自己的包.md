##通过composer发布自己的包
###首先创建一个自己的包
在github上创建一个项目，例如创建一个"new-composer"的项目。然后将项目同步到本地

```shell
git clone https://github.com/yourname/new-composer.git
cd new-composer
```
然后通过命令创建文明自己的composer.json文件。

```shell
composer init
//根据提示输入相关信息
Package name (<vendor>/<name>) [hou/composer-car]: 这里填写<包提供者>/<包名>的信息
Description []: 包的描述
Author [yourname <youremail>]: 作者信息
Minimum Stability []: 最低稳定版本，不知道的话，填写dev就可以。
License []: 授权协议，不知道的话添加MIT就可以
type[]: 根据项目填写type，不知道的话填写project就可以。
```
输入完之后，就会生成一个composer.json文件，文件内容如下：

```json
{
    "name": "包名称",
    "description": "",
    "type": "project",
    "license": "MIT",
	"keywords": [],
    "authors": [
        {
            "name": "yourname",
            "email": youremail"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}
//将其修改为：
{
    "name": "包名称",
    "description": "",
    "type": "project",
    "license": "MIT",
	"keywords": [],
    "authors": [
        {
            "name": "yourname",
            "email": youremail"
        }
    ],
    "minimum-stability": "dev",
    "require": {
		"php": ">=5.4.0"
    },
    "autoload": {
        "psr-4": {
            "namespace": "目录路径",//eg："Ford\\Escape\\": "src",
        }
    } 
}
```

项目结构如下：
>composer-car   
>- src  
>- .gitignore
- composer.json
>- README.md

这个时候项目已经部署完毕，然后安装：

```shell
composer install
```
运行成功后，你会发现目录中出现了一个vendor的目录，你可以自行查看目录文件，了解相关目录意义。

然后将自己的项目push都github上去。
###通过composer发布自己的包
到[packagist](https://packagist.org/)官网上去注册账号，或者通过github账号登录。

登录后，选择s导航栏的sublimt按钮，在Repository URL中填写你的github项目的路径。

点击check验证你的composer.json是否合格。没有出错的话，点击提交。

提交后，你的包就已经发布出去了。

配置github自动同步代码到packagist中。

使Packagist服务钩可以确保你的包裹总是推动GitHub时立即被更新。所以你可以去你的GitHub中进入需要同步的项目中,点击“Settings”按钮,然后“Webhooks & Services”。添加一个“Packagist”服务,并配置API令牌,加上你的Packagist用户名。检查“Active”框和提交表单。您可以点击“Test Service”按钮来触发它并检查如果Packagist删除警告包没有被自动更新。