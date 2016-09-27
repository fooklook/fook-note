##图片以base64格式储存时的问题及解决方案

###前端以base64格式形式显示图片

前端显示的base64时的格式

```html
<img src="data:image/jpg;base64,base64的编码">
```

###php将图片文件转换成base64格式

```php
$file_name = "";
//获取图片的mime类型
$mime_type = mime_content_type($file_name);
//显示图片
echo "<img src='data:".$mime_type.";base64,".base64_encode(file_get_contents($file_name))."'>";
```

###应用环境

随着富文本编辑器的多样化，很多灵活性的富文本编辑器，都没有提供图片上传的组件，比如：Wysiwyg。

因为这款富文本编辑器轻便灵活，还能兼顾不同分辨率的兼容性问题。

这款编辑器的图片上传功能并没有将图片上传到服务器上去，而是直接将图片以base64格式的文本显示在页面上。所以提交到服务器时，文本的内容就会过多。

首先就会面临这些问题：

####数据库储存

text格式最大长度为65535字节，LongText的长度为4294967295，大概就是4MB多。如果是一张高清照片的话，估计就放不下了。

####post提交数据限制

post提交数据限制大多数apache环境下限制为2MB，如果不修改配置文件，是无法提交post内容的。