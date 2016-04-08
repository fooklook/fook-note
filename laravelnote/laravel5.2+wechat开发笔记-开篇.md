##laravel5.2+wechat开发笔记

laravel拥有很多开发团队为其贡献代码，而微信公众账号这么火，所以自然也少不了很多团队为其开发对应的包。

我使用的就是[laravel-wechat](https://github.com/overtrue/laravel-wechat)的开发包,但该开发团队并没有为其写详细的开发文档。

还好我在他们的官网上找到了公共包的[使用文档](https://easywechat.org/docs/zh-cn/tutorial.html)，阅读后也能进行正常的开发。

###微信接入

要为自己的微信公众账号开启开发者模式，第一步就要做微信接入。即微信认证你的服务器。

####创建laravel项目

我使用的php版本为php5.5.9，所以使用laravel的版本是laravel5.2。laravel-wechat3.x版本的包的要求就是laravel>5.1,所以开发前请升级php版本。

```shell
composer create-project laravel/laravel easymall --prefer-dist
```

####引入laravel-wechat包

```shell
composer require "overtrue/laravel-wechat:~3.0"
```

####配置

在.env文章中进行对应的数据库等配置，我就不对说了。

####配置laravel-wechat包

config/app.php

```php
//注册 ServiceProvider
Overtrue\LaravelWechat\ServiceProvider::class,
//别名 aliases
'Wechat' => Overtrue\LaravelWechat\Facade::class,
```

创建配置文件

```php
php artisan vendor:publish
```

执行后，config文件中会出现wechat.php的配置文件。

然后在.env文件中添加以下字段，并配置对应的值

```env
WECHAT_APPID=
WECHAT_SECRET=
WECHAT_TOKEN=
WECHAT_AES_KEY=
```

创建控制器WechatController

```shell
php artisan make:controller WechatController
```

以下为控制器代码

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use EasyWeChat\Foundation\Application;
use App\Http\Requests;
use Illuminate\Support\Facades\Log;

class WechatController extends Controller
{
    //微信入口
    public function index(Application $wechat,Request $request){
        //Log::info(json_encode($request));
        $server = $wechat->server;
        $server->serve()->send();
    }
}
```

编写路由

```php
Route::any('wechat', 'WechatController@index');
```

将项目同步到服务器中，就可以通过微信接入验证了。

###消息自动回复

不多说，直接上代码

```php
class WechatController extends Controller
{
    //微信入口
    public function index(Application $wechat,Request $request){
        //Log::info(json_encode($request));
        $server = $wechat->server;
        $server->setMessageHandler(function($message){
            return '欢迎使用fooklook回复助手，功能等待完善中。';
        });
        $server->serve()->send();
    }
}
```