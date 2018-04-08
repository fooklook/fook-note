## PHP开发者最容易犯的错误

### 在 foreach循环后留下数组的引用

```php
$array = [1, 2, 3];
echo implode(',', $array), "\n";
foreach ($array as &$value) {}    // 通过引用遍历
echo implode(',', $array), "\n";
foreach ($array as $value) {}     // 通过赋值遍历
echo implode(',', $array), "\n";
//输出结果
1,2,3
1,2,3
1,2,2
```

你没有看错，最后一行的最后一个值是 2 ，而不是 3 ，为什么？

在完成第一个 foreach 遍历后， $array 并没有改变，但是像上述解释的那样， $value 留下了一个对 $array 最后一个元素的危险的引用（因为 foreach 通过引用获得 $value ）

这导致当运行到第二个 foreach ，这个"奇怪的东西"发生了。当 $value 通过赋值获得， foreach 按顺序复制每个 $array 的元素到 $value 时，第二个 foreach 里面的细节是这样的

- 第一步：复制 $array[0] （也就是 1 ）到 $value （$value 其实是 $array最后一个元素的引用，即 $array[2]），所以 $array[2] 现在等于 1。所以 $array 现在包含 [1, 2, 1]
- 第二步：复制 $array[1]（也就是 2 ）到 $value （ $array[2] 的引用），所以 $array[2] 现在等于 2。所以 $array 现在包含 [1, 2, 2]
- 第三步：复制 $array[2]（现在等于 2 ） 到 $value （ $array[2] 的引用），所以 $array[2] 现在等于 2 。所以 $array 现在包含 [1, 2, 2]

### isset,empty,is_null

isset

如果 变量 存在(非NULL)则返回 TRUE，否则返回 FALSE(包括未定义）。变量值设置为：null，返回也是false;unset一个变量后，变量被取消了。注意，isset对于NULL值变量，特殊处理。

is_null

检测传入值【值，变量，表达式】是否是null,只有一个变量定义了，且它的值是null，它才返回TRUE . 其它都返回 FALSE 【未定义变量传入后会出错！】. 

empty

如果 变量 是非空或非零的值，则 empty() 返回 FALSE。换句话说，""、0、"0"、NULL、FALSE、array()、var $var、未定义; 以及没有任何属性的对象都将被认为是空的，如果 var 为空，则返回 TRUE。

empty只能用来检测变量，如果用来检测函数返回值，会报错。

```php
//首先，让我们回到数组和 ArrayObject 实例（和数组类似）。考虑到他们的相似性，很容易假设它们的行为是相同的。然而，事实证明这是一个危险的假设。举例，在 PHP 5.0 中:
// PHP 5.0 或后续版本:
$array = [];
var_dump(empty($array));        // 输出 bool(true)
$array = new ArrayObject();
var_dump(empty($array));        // 输出 bool(false)
// 为什么这两种方法不产生相同的输出呢？
//更糟糕的是，PHP 5.0之前的结果可能是不同的：
// PHP 5.0 之前:
$array = [];
var_dump(empty($array));        // 输出 bool(false)
$array = new ArrayObject();
var_dump(empty($array));        // 输出 bool(false)
//为了避免这种情况，对数组和ArrayObject实例的判断，最好用count
$array = [];
var_dump(count($array));        // 输出 int(0)
$array = new ArrayObject();
var_dump(count($array));        // 输出 int(0)
```

```php
class Regular
{
    public $test = 'value';
}
class Magic
{
    private $values = ['test' => 'value'];

    public function __get($key)
    {
        if (isset($this->values[$key])) {
            return $this->values[$key];
        }
    }
}
$regular = new Regular();
var_dump($regular->test);    // 输出 string(4) "value"
$magic = new Magic();
var_dump($magic->test);      // 输出 string(4) "value"
var_dump(empty($regular->test));    // 输出 bool(false)
var_dump(empty($magic->test));      // 输出 bool(true)
```

### 认为 PHP 支持单字符数据类型

```php
for ($c = 'a'; $c <= 'z'; $c++) {
    echo $c . "\n";
}
```

它确实会输出 a 到 z，但是，它还会继续输出 aa 到 yz。

PHP 中没有 char 数据类型； 只能用 string 类型。记住一点，在 PHP 中增加 string 类型的 z 得到的是 aa：

```php
$c = 'z'; echo ++$c . "\n";
//输出aa
```

这也是为什么上面那段简单的代码会输出a到z然后继续 输出aa到yz。它停在了za，那是它遇到的第一个比z大的。

### 关于通过引用返回与通过值返回的困惑

```php
class Config
{
    private $values = [];
    public function getValues() {
        return $this->values;
    }
}
$config = new Config();
$config->getValues()['test'] = 'test';
echo $config->getValues()['test'];
//输出PHP Notice:  Undefined index: test in /path/to/my/script.php on line 21
class Config
{
    private $values;
    // 使用数组对象而不是数组
    public function __construct() {
        $this->values = new ArrayObject();
    }
    public function getValues() {
        return $this->values;
    }
}
$config = new Config();
$config->getValues()['test'] = 'test';
echo $config->getValues()['test'];
//这里输出时正常的
```

原因是，与数组不同，PHP永远会将对象按引用传递。