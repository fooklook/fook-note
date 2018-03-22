## php常用算法

### 冒泡排序（数组排序）

```php
function bubble_sort($array) { 
    $count = count($array); 
    if ($count <= 0 ) return false; 
    for($i=0 ; $i<$count; $i++){ 
            for($j=$count-1; $j>$i; $j--){ 
                    if ($array[$j] < $array[$j-1]){ 
                            $tmp = $array[$j]; 
                            $array[$j] = $array[$j-1]; 
                            $array[$j-1] = $tmp; 
                    } 
            } 
    } 
    return $array; 
}
```

### 快速排序（数组排序）

```php
function quick_sort($array) { 
        if (count($array) <= 1) return  $array; 
        $key = $array[0]; 
        $left_arr  = array(); 
        $right_arr = array(); 
        for ($i= 1; $i<count($array); $i++){ 
                if ($array[ $i] <= $key) 
                    $left_arr[] = $array[$i]; 
                else 
                    $right_arr[] = $array[$i]; 
        } 
        $left_arr = quick_sort($left_arr); 
        $right_arr = quick_sort($right_arr); 
        return array_merge($left_arr , array($key), $right_arr); 
}
```

### 斐波那契数列

简单递归算法

```php
function fibon($n)  
{  
    $n++;  
    if($n <= 2)//递归出口  
    {  
        return 1;  
    }  
    return fibon($n-1) + fibon($n-2);  
}  
```

#### 优化算法思路

通过记录原先计算的值，达到优化，递归计算。

优化后的递归函数

```php
function fibonac($a, $b, $n)  
{  
    $n++;  
    if($n > 2)  
    {  
        return fibonac($a+$b,$a,$n-1);  
    }  
    return $a;  
}
```

优化后的常规函数

```php
function fibonac($n){
    $array = array();
    if($n > 2)  
    {  
        $array[0] = 1;
        $array[1] = 1;
        for($i=2; $i<$n; $i++){
            $array[$i] = $array[$i-1] + $array[$i-2];
        }
        return $array[$n];
    }else{
        return 1;
    }
}
```

### 杨辉三角

```php
function yangHuiSanjiao($init = 1, $depth = 8)
{
    $arr = array( array($init));
    for($i = 0; $i < $depth; $i++)
    {
        for($j = 0; $j <= $i + 1; $j++) //获取 ‘下一行的数目’=‘上一行的数目’+1
        {
            $arr[$i + 1][$j] = $arr[$i][$j] + $arr[$i][$j - 1];
        }
    }
   
    return $arr;
}
```
