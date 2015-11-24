##laravel5学堂-路由
路由设置是laravel与以往的框架的一个很大不同点。为什么会这么设计，有兴趣的同学可以google一下RESTfull风格。

###声明路由
GET路由

```php
Route::get('routename',function(){});
//或者
Route::get('routename', 'action@controllername');
```

POST/PUT/DELETE路由与get类似

```php
Route::post('routename',function(){});
Route::put('routename',function(){});
Route::delete('routename',function(){});
```

多种请求注册路由

```php
Route::match(['get','post'], 'routename',function(){});
```

相应所有请求路由

```php
Route::any('routename',function(){});
```

路由群组

```php
//指定中间件
Route::group(['middleware' => ['foo', 'bar']],function(){
    Route::get('/', function(){});
});
//指定控制器的空间命名
Route::group(['namespace' => 'Admin',function(){
    Route::get('/', function(){});
});
//子域名路由
Route::group(['domain' => '{account}.myapp.com'], function(){});
//指定路由前缀
Route::group(['prefix' => 'admin'], function()
{
    Route::get('detail', function($account_id){}); //路由路径为admin/detail
});
```
###路由参数
基本用法

```php
Route::get('user/{name}', function($name = null)
{
    return $name;
});
//使用限定条件
Route::get('user/{name}', function($name)
{
    return $name;
})
->where('name', '[A-Za-z]+');
```
定义全局模式

```php
$router->pattern('id', '[0-9]+');
```
取得路由参数

```php
use Illuminate\Http\Request;
Route::get('user/{id}', function(Request $request, $id)
{
    if ($request->route('id')){}
});
```
路由群组

```php
Route::group([
    'prefix' => 'accounts/{account_id}',
    'where' => ['account_id' => '[0-9]+'],
], function() {

    // Define Routes Here
});
```

###路由别名
```php
Route::get('user/profile', ['as' => 'profile', function(){}]);
//OR
Route::get('user/profile', [
    'as' => 'profile', 'uses' => 'UserController@showProfile'
]);

```

###特殊用法
controller用法

```php
//单个
Route::controller('auth', 'Auth\AuthController',);
//多个
Route::controllers([
	'auth' => 'Auth\AuthController',
	'password' => 'Auth\PasswordController',
]);
//特别定义方式
Route::controller(
  'options',
  'OptionsController',
  [
    'getSite'=>'options.site'
  ]
);
// 现在就可以使用创建链接啦
URL::route('options.site')
```
该方法不需要路由中添加访问方式，直接在控制器中定义请求方式。

```php
class UserController extends Controller{
    public getIndex(){}
}
```
通过这种方式就能生成get的index@UserController的路由。(注意大小写)

resource方法(详细可以查看控制器章节)
这个方法也不需要事先声明路由。
它将自动生成CURD的相关路由。

###命令查询路由
路由列表，也就是你在routes.php文件中，命名的所有路由列表。

```shell
php artisan route:list
```

###路由缓存
使用路由缓存，将大幅降低注册应用程序所有路由所需要的时间。

生成路由缓存：

```php
php artisan route:cache
```
移除路由缓存，不产生新的缓存：

```php
php artisan route:clear
```

###警告
千万不要在routes.php文件定义多余的路由，这样会报错。
