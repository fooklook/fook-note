##PHP中PSR-[0-4]代码规范

###PHP-FIG

在说啥是PSR-[0-4]规范的之前，我觉得我们有必要说下它的发明者和规范者：PHP-FIG，它的网站是：www.php-fig.org。就是这个联盟组织发明和创造了PSR-[0-4]规范/

FIG 是 Framework Interoperability Group（框架可互用性小组）的缩写，由几位开源框架的开发者成立于 2009 年，从那开始也选取了很多其他成员进来，虽然不是 “官方” 组织，但也代表了社区中不小的一块。组织的目的在于：以最低程度的限制，来统一各个项目的编码规范，避免各家自行发展的风格阻碍了程序设计师开发的困扰，于是大伙发明和总结了PSR，PSR是Proposing a Standards Recommendation（提出标准建议）的缩写，截止到目前为止，总共有5套PSR规范，分别是：

```
PSR-0 (Autoloading Standard) 自动加载标准 
PSR-1 (Basic Coding Standard) 基础编码标准 
PSR-2 (Coding Style Guide) 编码风格向导 
PSR-3 (Logger Interface) 日志接口 
PSR-4 (Improved Autoloading) 自动加载的增强版，可以替换掉PSR-0了。
```

###PSR-0 规范（自动加载规范）

PSR-0规范是他们出的第1套规范，主要是制定了一些自动加载标准（Autoloading Standard）,但目前已经被已经被PSR-4代替，官网也不推荐使用。

PSR-0强制性要求几点：

- 一个完全合格的namespace和class必须符合这样的结构：“\< Vendor Name>(< Namespace>)*< Class Name>”
- 每个namespace必须有一个顶层的namespace（"Vendor Name"提供者名字）
- 每个namespace可以有多个子namespace
- 当从文件系统中加载时，每个namespace的分隔符(/)要转换成 DIRECTORY_SEPARATOR(操作系统路径分隔符)
- 在类名中，每个下划线(_)符号要转换成DIRECTORY_SEPARATOR(操作系统路径分隔符)。在namespace中，下划线(_)符号是没有（特殊）意义的。
- 当从文件系统中载入时，合格的namespace和class一定是以 .php 结尾的
- verdor name,namespaces,class名可以由大小写字母组合而成（大小写敏感的）

个人总结规范内容：

- 命名空间要和目录名对应,如下

```
vendor/
    vendor_name/
        package_name/
            src/
                Vendor_Name/
                    Package_Name/
                        ClassName.php       # Vendor_Name\Package_Name\ClassName
            tests/
                Vendor_Name/
                    Package_Name/
                        ClassNameTest.php   # Vendor_Name\Package_Name\ClassName
```

- 命名空间区分大小写
- 所有文件要以php结尾（apache是支持php3后缀的）


### PSR-1 规范（基本代码规范）

- PHP源文件必须只使用 <?php 和 <?= 这两种标签。
- 源文件中php代码的编码格式必须是不带字节顺序标记(BOM)的UTF-8。
- 一个源文件建议只用来做声明（类(class)，函数(function)，常量(constant)等）或者只用来做一些引起副作用的操作（例如：输出信息，修改.ini配置等），但不建议同时做这两件事。
- 命名空间(namespace)和类(class) 必须遵守PSR-0标准。
- 类名(class name) 必须使用骆驼式(StudlyCaps)写法 (注：驼峰式(cameCase)的一种变种，后文将直接用StudlyCaps表示)。
- 类(class)中的常量必须只由大写字母和下划线(_)组成。
- 方法名(method name) 必须使用驼峰式(cameCase)写法。

个人总结规范内容

- php文件的字符类型是无BOM头的UTF-8格式
- php代码只能用<?php ?>和 <?= ?>两种格式开头
- 一个php文件只做一件事情。
- 类名和方法名要使用驼峰命名，常量只能有大写字母和_组成

### PSR-2 规范（代码样式规范）

个人总结
- 文件末尾必须空一行。
- 必须使用Unix LF(换行)作为行结束符。
- 纯PHP代码源文件的关闭标签?>必须省略。
- 必须使用4个空格来缩进，不能使用Tab键。
- 一行推荐的是最多写80个字符，多于这个字符就应该换行了，一般的编辑器是可以设置的。
- php的关键字，必须小写，boolean值：true，false，null 也必须小写。

php关键字：'__halt_compiler', 'abstract', 'and', 'array', 'as', 'break', 'callable', 'case', 'catch', 'class', 'clone', 'const', 'continue', 'declare', 'default', 'die', 'do', 'echo', 'else', 'elseif', 'empty', 'enddeclare', 'endfor', 'endforeach', 'endif', 'endswitch', 'endwhile', 'eval', 'exit', 'extends', 'final', 'for', 'foreach', 'function', 'global', 'goto', 'if', 'implements', 'include', 'include_once', 'instanceof', 'insteadof', 'interface', 'isset', 'list', 'namespace', 'new', 'or', 'print', 'private', 'protected', 'public', 'require', 'require_once', 'return', 'static', 'switch', 'throw', 'trait', 'try', 'unset', 'use', 'var', 'while', 'xor'

- 命名空间(namespace)的声明后面必须有一行空行。
- 所有的导入(use)声明必须放在命名空间(namespace)声明的下面。
- 在导入(use)声明代码块后面必须有一行空行。
- 继承(extends) 和实现(implement) 必须和 class name 写在一行，切花括号要换行写。
- 属性(property)必须声明其可见性，到底是 public 还是 protected 还是 private，不能省略，也不能使用var, var是php老版本中的什么方式，等用于public。
- 方法(method)，必须 声明其可见性，到底是 public 还是 protected 还是 private，不能省略。并且，花括号{必须换行写。如果有多个参数，第一个参数后紧接, ,再加个空格，且函数name和( 之间必须要有个空格：function_name ($par, $par2, $pa3), 如果参数有默认值，也要用左右空格分开。
- 当用到抽象(abstract)和终结(final)来做类声明时，它们必须放在可见性声明 （public 还是protected还是private）的前面。而当用到静态(static)来做类声明时，则必须放在可见性声明的后面。
- 还有一些逻辑符的规范，这里就不一一举例。

### PSR-3 规范（日志接口规范）

PSR-3(Logger Interface)日志接口规范，主要目的是为了让日志类库通过接收一个 LoggerInterface 对象来记录日志信息。

1.LoggerInterface 接口对外定义了八个方法，分别用来记录 RFC 5424 中定义的八个等级的日志：debug、info、notice、warning、error、critical、alert、emergency

2.第九个方法 log()，第一个参数为记录等级。可使用一个预先定义的等级常量作为参数来调用此方法，必须与直接调用以上八个方法具有相同的效果。如果传入的等级常量没有预先定义，则必须抛出 psr\Log\InvalidArgumentException 类型的异常。不推荐使用自定义的日志等级，除非你非常确定当前类库对其有所支持。

### PSR-4 规范（自动加载规范，PSR-0 规范的升级版，更加简介）

- 废除了PSR-0中_就是目录分割符的写法，_下划线在完全限定类名中是没有特殊含义了。 
- 类文件名要以 .php 结尾。 
- 类名必须要和对应的文件名要一模一样，大小写也要一模一样。

目录结构风格改为：

```
vendor/
    vendor_name/
        package_name/
            src/
                ClassName.php       # Vendor_Name\Package_Name\ClassName
            tests/
                ClassNameTest.php   # Vendor_Name\Package_Name\ClassNameTest
```