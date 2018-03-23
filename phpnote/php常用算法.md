## php常用算法

### 排序算法

#### 冒泡排序

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

#### 快速排序

1. 从数列中挑出一个元素，称为 “基准”（pivot），
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作。
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

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

#### 插入排序

1. 从第一个元素开始，该元素可以认为已经被排序
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
3. 如果该元素（已排序）大于新元素，将该元素移到下一位置
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
5. 将新元素插入到该位置后
6. 重复步骤2~5

```php
function insertSort($arr) {
    $len=count($arr); 
    for($i=1, $i<$len; $i++) {
        $tmp = $arr[$i];
        //内层循环控制，比较并插入
        for($j=$i-1;$j>=0;$j--) {
            if($tmp < $arr[$j]) {
                //发现插入的元素要小，交换位置，将后边的元素与前面的元素互换
                $arr[$j+1] = $arr[$j];
                $arr[$j] = $tmp;
            } else {
                //如果碰到不需要移动的元素，由于是已经排序好是数组，则前面的就不需要再次比较了。
                break;
            }
        }
    }
    return $arr;
}
```

#### 二分插入排序

和插入排序相比，就是第3步，不是从最左边开始比较，而是从中间开始向两遍进行比较。

```php
// 分类 -------------- 内部比较排序
// 数据结构 ---------- 数组
// 最差时间复杂度 ---- O(n^2)
// 最优时间复杂度 ---- O(nlogn)
// 平均时间复杂度 ---- O(n^2)
// 所需辅助空间 ------ O(1)
// 稳定性 ------------ 稳定

function InsertionSortDichotomy(array $A, $n)
{
    for (int $i = 1; $i < n; $i++)
    {
        $get = $A[$i];                    // 右手抓到一张扑克牌
        $left = 0;                    // 拿在左手上的牌总是排序好的，所以可以用二分法
        $right = $i - 1;                // 手牌左右边界进行初始化
        while ($left <= $right)            // 采用二分法定位新牌的位置
        {
            $mid = ceil(($left + $right) / 2);
            if ($A[$mid] > $get)
                $right = $mid - 1;
            else
                $left = $mid + 1;
        }
        for (int $j = $i - 1; $j >= $left; j--)    // 将欲插入新牌位置右边的牌整体向右移动一个单位
        {
            $A[$j + 1] = $A[$j];
        }
        $A[$left] = $get;                    // 将抓到的牌插入手牌
    }
}
```

#### 堆排序heapsort 

### 斐波那契数列

斐波那契数列 1,1,2,3,5,8,13,21 后一个数是前两个数相加

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

![杨辉三角](./images/yanghuisanjiao.jpg)

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
