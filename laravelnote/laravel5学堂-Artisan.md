##laravel5学堂-Artisan
###Artisan有什么用
首先，Artisan自带了很多命令，已经具备了平时开发时，所需要的创建相关系统文件和快速部署项目的命令。这些命令可以减少很多边枝末节的开发任务，使你有更多的时间专注你的主要开发任务中来。

Artisan可以通过添加一个Cron对象到服务器，就激活整个项目的定时执行任务，将定时执行项目纳入到项目可控范围内。通常情况下，这类开发我们都是通过在服务器中添加多个定时任务，来实现，在项目迁移等操作中，定时器任务需要独立做一个迁移和调试的开发任务，使得项目的部署效率降低。

除了做定时任务外，我们可以自定义一些命令，用于检测项目的一个其他动作，比如外部数据请求测试。比如与银行对接的支付任务中，银行的开发者首先要保证的银行系统的稳定性，所以就不能像微信那样，做的比较灵活，当你没有确认支付成功的时候，它会不断的向你的借口推送支付成功的信息。这个时候就需要你这边主动去访问银行的接口，获取支付成功信息。

还有就是图片同步的问题，一个大体量的网站，用户上传的图片可能不计其数。处于项目安装的考虑，网站一般都要做备份。数据库和代码比较好处理，都有相关的成熟方案。但是对于图片，储存量比较大，人工处理的话，不难，但是时间成本比较高。如果单独开发一个系统进行管理，成本又比较高。就比如我现在项目图片会通过云存储进行备份和CDN加速服务，但租用的低成本云服务器不是特别的信任，可能在访问量大的时候，图片自动同步功能就会出现问题。所以我就自己写了一个命令去确定图片的同步状况。不管是定时任务自动确认，还是手动确认，效率都比较高。

###内置Artisan命令介绍
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

在通过命令创建command文件后，文件中有两个方法getArguments与getOptions。

理解这两个参数并不难，使用过命令行的同学应该都知道，通过传入参数实现不同的执行动作。

```php
//在getArguments方法中需要传入四个元素。
[$name, $mode, $description, $defaultValue]
//通过设置参数mode的值，设定该命令参数是否为必须数据参数。InputArgument::REQUIRED必须 InputArgument::OPTIONAL可选
//在getOptions方法中需要传入五个元素
[$name, $shortcut, $mode, $description, $defaultValue]
//通过设置参数mode的值，设定传入参数的类型。InputOption::VALUE_REQUIRED, InputOption::VALUE_OPTIONAL, InputOption::VALUE_IS_ARRAY, InputOption::VALUE_NONE。
```
4.获取输入的值

```php
//取得自定义命令被输入的参数
$value = $this->argument('name');
//取得自定义命令被输入的所有参数
$arguments = $this->argument();
//取得自定义命令被输入的选项
$options = $this->option();
```

5.输出信息到屏幕

```php
//显示一般消息到终端屏幕
$this->info('Display this on the screen');
//显示错误消息到终端屏幕
$this->error('Something went wrong!');
//提示用户进行输入
$name = $this->ask('What is your name?');
//提示用户进行加密输入
$password = $this->secret('What is the password?');
//提示用户进行确认
if ($this->confirm('Do you wish to continue? [yes|no]')){}
//指定默认输入
$this->confirm($question, true);
//调用其它命令
$this->call('command:name', ['argument' => 'foo', '--option' => 'bar']);
```