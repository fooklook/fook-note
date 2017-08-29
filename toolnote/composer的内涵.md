##composer的内涵

###composer文件加载方式

####files方式加载

指定需要加载的文件

```
//配置方式
{
    "autoload": {
        "files": ["path/filesname"]
    }
}
```

####classmap方式加载

将对应文件夹中的所有的class的 namespace + classname 生成成一个 key => value 的 php 数组,需要的时候进行对应加载。

```
//配置方式
{
  "classmap": ["src/"]
}
```

####psr-0

按照命名空间对应文件夹的方式，每当调用时，将会自动加载对应文件夹内的文件。

```
{
  "name": "acme/util",
  "auto" : {
    "psr-0": {
      "Acme\\Util\\": "src/"
    }
  }
}
```

文件结构

```
vendor/
  acme/
    util/
      composer.json
      src/
        Acme/
          Util/
            ClassName.php
```

ClassName.php 中是这样的

```
<?php
class Acme_Util_ClassName{}
?>
```

####psr-4

psr-4是psr-0的升级版，配置上没有什么区别，但是在文件目录上有区别。

```
{
  "name": "acme/util",
  "auto" : {
    "psr-4": {
      "Acme\\Util\\": "src/"
    }
  }
}
```

文件结构

```
vendor/
  acme/
    util/
      composer.json
      src/
        ClassName.php
```

ClassName.php 中是这样的

```
<?php
namespace Acme\Util;
class ClassName {}
?>
```

需要注意的是，在修改这些配置之后，需要执行composer dump-autoload命令。特别是files和classmap，如果不执行会报错。psr-4和psr-0虽然不会报错，但是代码运行的效率会降低。

具体加载逻辑下：

- 找 Composer\ClassLoader 如果不存在就是生成一个实例放在 ComposerAutoloaderInit64c47026c93126586e44d036738c0862 中
- 然后将 composer cli 生成的各种 autoload_psr4, autoload_classmap, autoload_namespaces(psr-0) 全都注册到 Composer\ClassLoader 中。
- 直接 require 所有在 autoload_files 中的文件
