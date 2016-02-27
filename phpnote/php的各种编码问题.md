##php的各种编码问题
现在的字符编码有很多中，在网站中主要用到的是GBK和UTF-8。对于phper来说，很多网站信息都是抓取其它网站内容或者文件获得的，所以会出现各种编码上的问题。
###判断字符编码
```php
$charset = mb_detect_encoding($str,array('UTF-8','GBK','GB2312')); 
if($charset == 'cp936'){
	$charset='GBK'; 
}
```
###将字符转换成UTF-8
```php
if("utf-8" != $charset){
	$str = iconv($charset,"UTF-8//IGNORE",$str);
} 
```