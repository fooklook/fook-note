##composer国内速度慢解决方案

###安装composer慢

####传统的安装方式

```
//下载composer
curl -sS https://getcomposer.org/installer | php
//设置全局命令
sudo mv composer.phar /usr/local/bin/composer
```

这种方法是官方提供的linux环境下安装的方式，但是国内在下载环节会比较慢，甚至失败。

####新方案安装

```
//下载composer-setup.php，即安装器。
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
//执行安装过程。
php composer-setup.php
//删除安装脚本。
php -r "unlink('composer-setup.php');"
//设置全局命令
sudo mv composer.phar /usr/local/bin/composer
```

上述方式安装，全称秒开。

###使用国内镜像

- 通过命令行，修改composer的全局配置文件

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

- 手动编辑composer全局配置文件

```
//通过命令查看composer配置
composer config -l -g
//在输出中[home]，可以看到composer的根目录，进入目录后，编辑config.json文件
 {
    "config": {},
    "repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://packagist.phpcomposer.com"
        }
    }
}
```

- 修改当前项目的composer.json配置文件,使得当前项目使用国内镜像

```
composer config repo.packagist composer https://packagist.phpcomposer.com
```

上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息:

```
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```