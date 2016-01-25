##laravel5学堂-中间件
![laravel流程图](http://7xo7bi.com1.z0.glb.clouddn.com/ycz_onion.png)

如上图所示，中间件就是处理session和Authentication这俩层事务。

###创建中间件

```shell
php artisan make:middleware MyMiddleware
```

###Before / After 中间件
分析中间件的操作

```php
//Before中间件
namespace App\Http\Middleware;
use Closure;
class AuthMiddleware {
    public function handle($request, Closure $next)
    {
	//进行逻辑判断和相关操作
        return $next($request);
    }

}
//After中间件
namespace App\Http\Middleware;
use Closure;
class AuthMiddleware {
    public function handle($request, Closure $next)
    {
        $response = $next($request);
        //进行逻辑判断和相关操作
        return $response;
    }

}
```
理解上很简单，$next($request)就是执行框架的下一步操作，Before中间件就是在下一步操作前进行逻辑处理，而After中间件，就是在处理完下一步处理后，再处理中间件逻辑。=_= 一开始我理解错了。

###使用中间件
####全局中间件
在app/Http/Kernel.php文件中$middleware变量中，添加相应的清单。这里的中间件，将被所有的HTTP请求执行。

####部分中间件
首先，在app/Http/Kernel.php文件中$routeMiddleware变量中，添加相应的清单。

```php
//在路由中指派中间件
Route::get('admin/profile', ['middleware' => 'auth', function(){}]);
//在控制器中指派路由
class HomeController extends Controller {
	public function __construct()
	{
		$this->middleware('auth');
		$this->middleware('log', ['only' => ['fooAction', 'barAction']]);
		$this->middleware('subscribed', ['except' => ['fooAction', 'barAction']]);
	}
	public function index()
	{
		return view('home');
	}

}
```