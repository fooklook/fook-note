##laravel5.2+新浪微博登录

###获取微博应用App Key和App Secret

通过自己的新浪微博创建应用，并获取到App Key和App Secret。

验证可能需要一些时间，不过可以在为审核的应用中添加测试账号，从而可以正常使用登录功能。

###安装socialiteproviders/weibo扩展包

```shell
composer require socialiteproviders/weibo
```

###项目相关配置

####添加 Service Provider

如果事先创建了Laravel\Socialite\SocialiteServiceProvider服务要删除掉。

```php
'providers' => [
//    Laravel\Socialite\SocialiteServiceProvider::class,
    SocialiteProviders\Manager\ServiceProvider::class, // add
],
```

####添加事件监听

在app\Providers\EventServiceProvider.php中的$listen属性添加以下内容

```php
protected $listen = [
    'SocialiteProviders\Manager\SocialiteWasCalled' => [
        'SocialiteProviders\Weibo\WeiboExtendSocialite@handle',
    ],
];
```

####修改config/services.php

```php
'weibo' => [
    'client_id' => env('WEIBO_KEY'),
    'client_secret' => env('WEIBO_SECRET'),
    'redirect' => env('WEIBO_REDIRECT_URI'),
],
```

####修改.env

添加一下环境变量

```php
WEIBO_KEY=
WEIBO_SECRET=
WEIBO_REDIRECT_URI=
```

###代码实现

####创建路由

因为laravel5.2默认不经过web中间件，所以要将路由写在web的路由群组中

```php
Route::group(['middleware' => ['web']], function () {
    // 引导用户到新浪微博的登录授权页面
    Route::get('auth/weibo', 'Auth\AuthController@weibo');
    // 用户授权后新浪微博回调的页面
    Route::get('auth/callback', 'Auth\AuthController@callback');
});
```

####创建控制器

在Auth\AuthController控制器中添加如下方法：

```php
public function weibo(Request $request) {
    return \Socialite::driver('weibo')->redirect();
    // return \Socialite::with('weibo')->scopes(array('email'))->redirect();
}


public function callback(Request $request) {
    $user = \Socialite::driver('weibo')->user();
    dd($user);
}
```
