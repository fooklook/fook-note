##laravel5学堂-请求及响应
请求及响应这个概念，并不陌生，所以，我这里写成一篇。主要提及一些函数，心里有个概念。

###获取请求实例
在路由中获取

```php
//获取输入值
$name = Request::input('name');		//事先需要引入命名空间
//获取设置默认值
$name = Request::input('name', 'Sally');
//判断是否有输入数据
Request::has('name');
//获取全部数据
Request::all();
//获取部分请求数据
$input = Request::only('username', 'password');
$input = Request::except('credit_card');
//通过点语法获取数据值
Request::input('products.0.name');
```
在控制器中，通过依赖注入方式获取

```php
public function store(Request $request)
{
    $name = $request->input('name');
}
```
###将数据写入到session中
在注册的过程中，可能需要多步，分步写入到数据库当中的方法可能会影响代码逻辑，所以事先存储在session中是一个比较好的方法。

将当前的输入数据存进session中。

```php
//全部保存
Request::flash();
//将部分输入数据存成一次性session
Request::flashOnly('username', 'email');
Request::flashExcept('password');
//快闪数据
redirect('url')->withInput();
redirect('url')->withInput(Request::except('password'));
```

取出旧数据

```php
Request::old('username');
```

###上传文件
取得上传文件

```php
//确认文件是否有上传
if (Request::hasFile('photo')){
    //确认上传数据是否有效
    if(Request::file('photo')->isValid())){
	//获取上传数据
        $file = Request::file('photo');	
	//移动上传文件
	Request::file('photo')->move($destinationPath);
	Request::file('photo')->move($destinationPath, $fileName);
    }
}
```
###其他请求数据

```php
//请求的路径
$url = Request::path();
//请求的URL
Request::url();
//是否使用ajax
if(Request::ajax()){}
//获取请求方法
$method = Request::method();
//判断请求方法
Request::isMethod('post');
//确认请求路径是否符合特定格式
Request::is('admin/*');
```

###响应
在路由中直接响应

```php
Route::get('/', function(){return 'Hello World';});
```
自定义响应

```php
//这个有点牛哟，以前要写好多代码，比如到导出csv文件，
return (new Response($content, $status))->header('Content-Type', $value);
```
重定向

```php
return redirect('user/login');
//定向到前一个Url
return redirect()->back();
return redirect()->back()->withInput();
//根据路由名称重定向
return redirect()->route('login');
//对路由进行赋值
return redirect()->route('profile', [1]);	//路由的 URI 为：profile/{id}
return redirect()->route('profile', ['id' => 1]);
//根据控制器action进行重定向
return redirect()->action('App\Http\Controllers\HomeController@index');
//对控制器进行赋值
return redirect()->action('App\Http\Controllers\UserController@profile', [1]);
return redirect()->action('App\Http\Controllers\UserController@profile', ['user' => 1]);
```
其它响应

```php
//JSON响应
return response()->json(['name' => 'Abigail', 'state' => 'CA']);
//JSONP响应
return response()->json(['name' => 'Abigail', 'state' => 'CA'])->setCallback($request->input('callback'));
//下载响应
return response()->download($pathToFile);
return response()->download($pathToFile, $name, $headers);
return response()->download($pathToFile)->deleteFileAfterSend(true);	//下载后删除
```
