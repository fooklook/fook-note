##css3渐变

###线性渐变

####不同浏览器兼容问题

```html
//正常浏览器使用方法
background:-moz-linear-gradient([<point> || <angle>]+,<stop>, <stop> [, <stop>]*);
background:-webkit-linear-gradient([<point> || <angle>]+,<stop>, <stop> [, <stop>]*);
background:-o-linear-gradient([<point> || <angle>]+,<stop>, <stop> [, <stop>]*);

//IE依靠滤镜实现渐变。startColorstr表示起点的颜色，endColorstr表示终点颜色。GradientType表示渐变类型，0为缺省值，表示垂直渐变，1表示水平渐变。
filter: progid:DXImageTransform.Microsoft.gradient(GradientType=0, startColorstr=#1471da, endColorstr=#1C85FB);/*IE<9>*/
-ms-filter: "progid:DXImageTransform.Microsoft.gradient (GradientType=0, startColorstr=#1471da, endColorstr=#1C85FB)";/*IE8+*/
```

####按point来指定渐变方向

```html
//从左向右渐变
background:-moz-linear-gradient(left,#000,#fff);
//从左上角向右下角渐变
background:-moz-linear-gradient(left top,#000,#fff);
```

####按angle（角度）指定渐变方向

```html
//从左向右渐变
background:-moz-linear-gradient(0deg,#000,#fff);
//从左下角向右上角渐变
background:-moz-linear-gradient(45deg,#000,#fff);
```

####指定渐变宽度

```html
//从左向右20%，80%进行渐变
background:-moz-linear-gradient(left,#000,20%,#fff);
//从左向右80%，20%进行渐变
background:-moz-linear-gradient(left,#000,80%,#fff);
```

####透明色+背景图片渐变

```html
background:-moz-linear-gradient(45deg,rgba(0,0,0,0),90%, rgba(255,0,255,1)),url(./001.jpg);
```

####代码示例

```html
<style type="text/css">
    .line-box{
        width:500px;
        height: 450px;
        background: -webkit-linear-gradient(left, red, #f96, yellow, green, #ace);
        background: -o-linear-gradient(left, red, #f96, yellow, green, #ace);
        background: linear-gradient(to right, red, #f96, yellow, green, #ace);
    }	
</style>
<div class="line-box"></div>
```

![线性渐变](./images/gradient001.png)

###经向渐变

IE10+等浏览器支持径向渐变

####不同浏览器写法

```html
-moz-radial-gradient([<bg-position> || <angle>]?, [<shape> || <size>]?, <color-stop>, <color-stop>[, <color-stop>]*); /* Firefox 3.6 - 15 */
-webkit-radial-gradient([<bg-position> || <angle>]?, [<shape> || <size>]?, <color-stop>, <color-stop>[, <color-stop>]*); /* Safari 5.1 - 6.0 */
-o-radial-gradient([<bg-position> || <angle>]?, [<shape> || <size>]?, <color-stop>, <color-stop>[, <color-stop>]*); /* Opera 11.6 - 12.0 */
radial-gradient([<bg-position> || <angle>]?, [<shape> || <size>]?, <color-stop>, <color-stop>[, <color-stop>]*); /* 标准的语法 */
```

