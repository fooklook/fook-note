##Form对象属性

###acceptCharset

服务器可接受的字符集。

常用值：

- UTF-8 - Unicode 字符编码
- ISO-8859-1 - 拉丁字母表的字符编码。
- gb2312 - 简体中文字符集

###action 

设置或返回表单提交的url地址

###enctype

设置或返回表单用来编码内容的 MIME 类型。

application/x-www-form-urlencoded： 在发送前编码所有字符（默认）。这是标准的编码格式。

multipart/form-data： 不对字符编码，在使用包含文件上传控件的表单时，必须使用该值。

text/plain： 窗体数据以纯文本形式进行编码，其中不含任何控件或格式字符。 

###id
设置或返回表单的 id。

###length

返回表单中的元素数目。
###method

设置或返回将数据发送到服务器的 HTTP 方法。

post

get

###name

设置或返回表单的名称。

###target

设置或返回表单提交结果的 Frame 或 Window 名。

