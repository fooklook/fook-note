##sassnote
###导入
关键字@import  
###变量
1. 普通变量  
声明方式：$fontSize = 12px;
2. 默认变量
声明方式：$fontSize;   12px !default;
3. 特殊变量
>
```sass
$borderDirection:       top !default; 
$baseFontSize:          12px !default;
$baseLineHeight:        1.5 !default;
.border-#{$borderDirection}{
  border-#{$borderDirection}:1px solid #ccc;
}
body{
    font:#{$baseFontSize}/#{$baseLineHeight};
}
.border-top{
  border-top:1px solid #ccc;
}
body {
  font: 12px/1.5;
}
```
4. 多值变量
> list  
声明list变量：  
$px: 5px 10px 20px 30px; //一维数组  
$px: (5px 10px) (10px 20px); //二维数组  
nth($list, $index)取值  
length($list)获取数组的长度
join($list1,$list2)合并数组  
append($list1,$list2)

> map以键值对的形式出现
eg: 

```sass
$map : (key1 : value1, key2 : value2, key3 : value3);
map-get($map,'key1');//获取值  
@each $key, $value in $map{
  #{$key}{
	font-size:$value;
  }
}
```

###嵌套
####选择器嵌套
所谓选择器嵌套指的是在一个选择器中嵌套另一个选择器来实现继承。  

```sass
body{
	color:red;
	.container{
		font-size:14px;
		&:hover{//&表示父元素选择器
			border:1px solid #000;
		}
	}
}
```
####属性嵌套
所谓属性嵌套值的是有些属性拥有同一个开头单词，如border-width,border-color。  

```sass
.container{
	border:{
		style:solid;
	}
}
```
###混合(mixin)
普通用法

```sass
@mixin center-block {
	margin-left:auto;
	margin-right:auto;
}
.demo{
	@include center-block;
}
```
参数用法

```sass
$mixin opacity($opacity:50){//默认为50
	opacity: $opacity/100;
}
.opacity{
	@include opactiy(100);
}
```
###继承(extend)

```sass
h1{ border: 4px solid #000;}
.speaker{
	@extend h1;
	border-width: 2px;
}
//生成的css style
h1, .speaker{ border: 4px solid #000;}
.speaker{ border-width:2px; }
```
###条件判断及循环

```sass
//判断
$type : true;
p{
	@if $type {
		color: blue;	
	}
}
//三目判断
if(true, 1px, 2px);
//for循环
@for $i from 1 through 3{}
//each循环
$list: value1, value2,value3;
@each $value in $list {}
$list: (value1,value2,value3), (value12,value13,value14);
@each $value1, $value2, $value3 in $list{}
$list: (key1:value1, key2:value2, key3:value3);
@each $key,$value in $list{}
```