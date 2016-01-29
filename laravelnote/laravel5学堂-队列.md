##laravel5学堂-队列
队列可以帮助我们处理一些异步事务，比如群发邮件等功能。如果同时处理可能会占用服务器过多的资源。

###配置
配置config/queue.php文件：

```php
'default' => 'database'
```
default的默认配置为sync为同步队列。改为database作为驱动，队列任务将会存放在数据库中，而后我们在后台启动一个后台服务用来监听，这样就做到了异步处理。

###创建数据库：

```shell
//在database/migrations目录中生成一个创建jobs表的配置文件。
php artisan queue:table
//生成jobs表
php artisan migrate
```
如果你在database/migrations目录中发现已经存在jobs表，那就不需要再执行这一步。

###添加队列任务
通过命令创建任务文件

```shell
php artisan make:command SendEmail --queued
```
执行命令后，你就会发现在app/Commands目录下多出一个SeedEmail.php文件。

```php
namespace App\Commands;
use App\Commands\Command;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Bus\SelfHandling;
use Illuminate\Contracts\Queue\ShouldBeQueued;
use Illuminate\Support\Facades\Mail;
class SendEmail extends Command implements SelfHandling, ShouldBeQueued {

	use InteractsWithQueue, SerializesModels;

	public $register;

	/**
	 * Create a new command instance.
	 *
	 * @return void
	 */
	public function __construct()
	{
		//$this->register = $register;
	}

	/**
	 * Execute the command.
	 *
	 * @return void
	 */
	public function handle()
	{
		//$register = $this->register;
		//发送邮件
		Mail::send('welcome', [], function($message){
			$message->to('to mail')->subject('邮箱验证');
		});
	}

}
```

###创建后台监听任务

在windows环境下，不方便创建后台任务，但我们可以使用git-bash后者windows的cmd执行一个任务一直监听这个任务。

```shell
php artisan queue:listen
```

如果在linux下则可以让监听任务在后台运行。

```shell
nohup php artisan queue:listen &
```

如果你觉得马上任务就被执行了，没有体现出异步处理的感觉，你可以添加睡眠时间。

```shell
php artisan queue:listen --sleep=10
```
任务将在10秒后执行