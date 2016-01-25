##laravel5学堂-控制器
学过框架的同学对于控制器应该不默认，但是在编写控制器代码的时候，心里默念一遍控制器的作用是什么。

Controller（控制器）是应用程序中处理用户交互的部分。
通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据

生成控制器的命令：

```php
//创建一个空的控制器
php artisan make:controller NameController --plain
//创建一个资源控制器，控制器包含CURD的action。
php artisan make:controller NameController
```

###接收变量
####接收路由变量
路由设置

```php
Route::get('user/{id}', 'UserController@showProfile');
```
控制器代码

```php
class UserController extends Controller {
    public function showProfile($id){}
}
```
####接收表单变量
用户通过表单提交上来的变量，通过依赖注入的方式，传递给action。

```php
class UserController extends Controller {
    public function showProfile($id, Request $request){}
}
```
$request就是表单提交上的变量。你可以通过$request->input['name']的方式，获取表单值。也可以通过$reuqest->name直接获取。

####获取正在执行的控制器行为名称
$action = Route::currentRouteAction();

####隐式控制器
对于偷懒的人来说，这种方式还是比较方便的。

定义路由

```php
Route::controller('users', 'UserController');
```
定义控制器

```php
class UserController extends Controller {
    //路由为GET请求 users/index
    public function getIndex(){}
    //路由为POST请求 users/profile
    public function postProfile(){}
    //路由为相应所有请求 users/login
    public function anyLogin(){}
}
```
####RESTful资源控制器
上面已经介绍到创建资源控制器的命令。

现在注册一个指向资源控制器的路由：

```php
Route::resource('photo', 'PhotoController');
```
注册后，查看路由列表命令。

```shell
php artisan route:list
```
你将发现，在路由列表中，出现了很多的新的路由。
