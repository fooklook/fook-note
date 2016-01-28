##laravel5学堂-邮件配置
最近测试邮件发送功能时，遇到一个坑，希望大家避免。以下是建议配置：
###配置.env文件
在项目的根目录找到.env文件，如果不存在则复制.env.example并将其给名为.env。

打开文件后，从英文名的意思，你就知道大概什么意思了，忽略其它配置项，直接编写邮件配置：

```php
MAIL_DRIVER=smtp
MAIL_HOST=smtp.163.com
MAIL_PORT=25
MAIL_USERNAME=你的163箱邮箱地址
MAIL_PASSWORD=163邮箱地址密码
```

接着配置app/mail.php文件。

如果你使用的是25端口，记得一定要把encryption改为null,不然邮件无法发送。

关于from项的配置，address一定要配置正确，也就是你的邮箱地址。而name则不能为空，但可随意配置，因为最后发件人已经配邮件服务器规定好了

```php
'driver' => env('MAIL_DRIVER', 'smtp'),
'host' => env('MAIL_HOST', 'smtp.mailgun.org'),
'port' => env('MAIL_PORT', 587),
'from' => ['address' => "", 'name' => ""],
'encryption' => null,				//加密方式，可以是tls(对应端口587)和ssl(对应端口465)
'username' => env('MAIL_USERNAME'),
'password' => env('MAIL_PASSWORD'),
'sendmail' => '/usr/sbin/sendmail -bs',
'pretend' => false,				//用于配置是否将邮件发送记录到日志中，默认为 false 则发送邮件不记录日志，如果为 true 的话只记录日志不发送邮件，这一配置在本地开发中调试时很有用。
```

发送邮件代码：第一个参数为邮件视图的名称。第二个是传递给该视图的数据，通常是一个关联式数组，让视图可通过 $key 来取得数据对象。第三个参数是一个闭包，可以对 message 进行各种配置。
send的参数配置：邮件视图
```php
    $flag = Mail::send('welcome',[],function($message){
      $to = '';	//测试发送的邮箱地址
      $message ->to($to)->subject('测试邮件');
    });
    if($flag){
      echo '发送邮件成功，请查收！';
    }else{
      echo '发送邮件失败，请重试！';
    }
```