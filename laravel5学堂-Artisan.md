##laravel5学堂-Artisan
Artisan是我提到的从文件式管理模式转为命令行管理模式的核心之一。

首先它自带了很多种命令，这些命令基本上覆盖了，你所有后台开发任务执行的动作。

```php
//通过这个命令，查看当前项目中所有可运行的命令。
php artisan
//或者
php artisan list
//或者
php artisan --ansi
```

以下是artisan常用命令功能的中文介绍  

```shell
Options:  
 --help (-h)	       显示帮助信息  
 --version (-V)        显示当前laravel版本  
 --ansi                该命令能和list命令差不多，不过它能显示出对比色。  
可用命令:
 clear-compiled       删除编译后的类文件
 down                 项目进入维护模式
 env                  显示当前框架环境
 fresh                移除脚手架中包含的框架 ?
 help                 帮助
 inspire              显示名言 =_=||
 list                 命令行列表
 migrate              执行数据库迁移
 optimize             优化框架
 serve                PHP开发服务器上的应用程序服务
 tinker               与应用程序交互
 up                   开启项目
app
 app:name             设置应用程序的命名空间
auth
 auth:clear-resets    刷新过期密码重置令牌
cache
 cache:clear          刷新应用程序缓存
 cache:table          为数据库缓存创建一个数据表
config
 config:cache         创建配置缓存
 config:clear         删除配置缓存
db
 db:seed              填充数据库
event
 event:generate       根据注册生成失踪事件和处理程序 ?
handler
 handler:command      创建一个新的命令处理程序类
 handler:event        创建一个新的事件处理程序类
key
 key:generate         生成一个app_key
make
 make:command         创建一个新命令类 ?
 make:console         创建一个新的命令 ?
 make:controller      创建一个控制器
 make:event           创建一个新的事件
 make:middleware      创建一个新的中间件
 make:migration       创建一个迁移文件
 make:model           创建一个模型
 make:provider        创建一个服务提供者
 make:request         创建一个表单请求类
migrate
 migrate:install      创建迁移数据库
 migrate:refresh      重置所有数据迁移
 migrate:reset        回滚所有数据库迁移
 migrate:rollback     回滚数据库迁移
 migrate:status       显示向上/向下迁移的列表
queue ?
 queue:failed         列出所有队列失败的工作
 queue:failed-table   为失败的队列工作创建一个数据库表
 queue:flush          重置所有的失败队列工作
 queue:forget         删除一个失败的队列工作
 queue:listen         监听一个队列
 queue:restart        重新启动队列工人守护进程在他们目前的工作
 queue:retry          重试队列失败的工作
 queue:subscribe      订阅一个铁的URL。io推动队列
 queue:table          为队列工作创建一个数据库表
 queue:work           处理队列的下一份工作
route
 route:cache          创建路由缓存
 route:clear          清除路由缓存文件
 route:list           列出所有路由
schedule
 schedule:run         运行调度命令
session
 session:table        为session创建一个回滚数据表
vendor
 vendor:publish       列出发布者提供的包
```
###在程序中调用命令行接口

```php
Route::get('/foo', function()
{
    $exitCode = Artisan::call('command:name', ['--option' => 'foo']);
});
```
###定时执行Artisan命令
首先在服务器上创建一个Cron对象。

```shell
* * * * * php /path/to/artisan schedule:run 1>> /dev/null 2>&1
```
该Cron对象没分钟执行一次。

然后在app/Console/Kernel.php的schedule方法中，添加你需要定时执行的schedule对象。

```php
//调用闭包
$schedule->call(function()
{
    // 执行一些任务...
})->hourly();
//调用终端命令
$schedule->exec('composer self-update')->daily();
//自己配置Cron表达式
$schedule->command('foo')->cron('* * * * *');
//频繁的工作
$schedule->command('foo')->everyFiveMinutes();
$schedule->command('foo')->everyTenMinutes();
$schedule->command('foo')->everyThirtyMinutes();
//每天工作
$schedule->command('foo')->daily();
//每天一次特定时间的工作
$schedule->command('foo')->dailyAt('15:00');
//每天两次的工作
$schedule->command('foo')->weekdays();
//每周一次的工作
$schedule->command('foo')->weekly();
// 调用每周一次在特定的日子 (0-6) 和时间的工作...
$schedule->command('foo')->weeklyOn(1, '8:00');
//每月一次的工作
$schedule->command('foo')->monthly();
//特定日期的工作
$schedule->command('foo')->mondays();
$schedule->command('foo')->tuesdays();
$schedule->command('foo')->wednesdays();
$schedule->command('foo')->thursdays();
$schedule->command('foo')->fridays();
$schedule->command('foo')->saturdays();
$schedule->command('foo')->sundays();
//防止工作重叠
$schedule->command('foo')->withoutOverlapping();
//限制应该执行工作的环境
$schedule->command('foo')->monthly()->environments('production');
//指定工作在当应用程序处于维护模式也应该执行
$schedule->command('foo')->monthly()->evenInMaintenanceMode();
//只允许工作在闭包返回 true 的时候执行
$schedule->command('foo')->monthly()->when(function()
{
    return true;
});
//将预定工作的输出发送到指定的 E-mail。你必须先把输出存到文件中才可以发送 email。
$schedule->command('foo')->sendOutputTo($filePath)->emailOutputTo('foo@example.com');
//将预定工作的输出发送到指定的路径
$schedule->command('foo')->sendOutputTo($filePath);
//在预定工作执行之后 Ping 一个给定的 URL。使用前必须引入GUZZLE HTTP库。在composer.json文件中添加。
$schedule->command('foo')->thenPing($url);
```
###Artisan开发
1.创建一个命令类

```shell
php artisan make:console FooCommand
```
该命令将在app/console/Commands文件下，创建一个FooCommand.php文件。

```shell
php artisan make:console FooCommand --command=foo:action
```
通过command选项自定义在终端中使用的命令。

在FooCommand.php文件中，修改命令描述和命令执行的动作。

2.注册自定义命令。  
在app/console/kernel.php文件中$commands变量中添加对应的文件路径。

3.命令参数与选项

...