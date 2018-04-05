## PHP弱类型引发的漏洞实例 

我们知道PHP 是一门弱类型语言，不必向 PHP 声明该变量的数据类型，PHP 会根据变量的值，自动把变量的值转换为正确的数据类型，但在这个转换过程中就有可能引发一些安全问题。

### 类型转换

1. 会先进行类型转换，再进行对比
2. 会先比较类型，如果类型不同直接返回false，参考如下

```php
$a = null;
$b = 'a';
$c = 0;
$a == $b;       //false
$a === $b;      //false
$a == $c;       //true
$a === $c;      //false
$b == $c;       //ture
$b === $c;      //false
```

### 函数松散性

#### switch()

如果switch是数字类型的case的判断时，switch会将其中的参数转换为int类型。

```php
$string = '2i';
switch($string){
    case 1:
        echo 1;
        break;
    case 2:
        echo 2;
        break;
    case '2i':
        echo '2i';
        break;
}
//输出2
```

#### is_number()

is_numeric在做判断时候，如果攻击者把payload改成十六进制0x…，is_numeric会先对十六进制做类型判断，十六进制被判断为数字型为真，就进入了条件语句，如果再把这个代入进入sql语句进入mysql数据库，mysql数据库会对hex进行解析成字符串存入到数据库中，如果这个字段再被取出来二次利用，就可能造成二次注入漏洞。

```php
$id = '0x3120616E6420313D31';  // 1 and 1=1到HEX
if(is_numeric($id)){
    $sql = 'select ....';
}
```

#### strcmp()

strcmp(string1，string2):比较括号内的两个字符串string1和string2，当他们两个相等时，返回0；string1的大于string2时，返回>0;小于时返回<0。在5.3及以后的php版本中，当strcmp()括号内是一个数组与字符串比较时，也会返回0。

#### md5()

string md5 ( string $str [, bool $raw_output = false ] )

md5()需要是一个string类型的参数。但是当你传递一个array时，md5()不会报错，只是会无法正确地求出array的md5值，返回null，这样就会导致任意2个array的md5值都会相等。