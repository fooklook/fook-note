##laravel5学堂-系统架构
在学习laravel5框架的时候，其它的东西都很容易就能理解，框架大同小异。

但是从laravel5的文档中学习到系统架构这里的时候，就被文档里面的介绍，弄蒙圈了。这几天看了很多别人写的博客，还算有所收获，所以总结下面内容。

这是一个简单的Laravel hashing服务

定义一个Contracts(协议)： Illuminate/Contracts/Hashing/Hasher.php

```php
<?php namespace Illuminate\Contracts\Hashing;
interface Hasher {
	/**
	 * Hash the given value.
	 *
	 * @param  string  $value
	 * @param  array   $options
	 * @return string
	 */
	public function make($value, array $options = array());
	/**
	 * Check the given plain value against a hash.
	 *
	 * @param  string  $value
	 * @param  string  $hashedValue
	 * @param  array   $options
	 * @return bool
	 */
	public function check($value, $hashedValue, array $options = array());
	/**
	 * Check if the given hash has been hashed using the given options.
	 *
	 * @param  string  $hashedValue
	 * @param  array   $options
	 * @return bool
	 */
	public function needsRehash($hashedValue, array $options = array());
}
```
Contracts 只是描述一个服务的具体方法，而不是实现该服务。

Container 才是服务的具体实现，对于一个服务来说，可能会有多种不同的实现。Hash目前只有一个，就是：Illuminate/Hashing/BcryptHasher.php

```php
<?php namespace Illuminate\Hashing;
use RuntimeException;
use Illuminate\Contracts\Hashing\Hasher as HasherContract;
class BcryptHasher implements HasherContract {
	/**
	 * Default crypt cost factor.
	 *
	 * @var int
	 */
	protected $rounds = 10;
	/**
	 * Hash the given value.
	 *
	 * @param  string  $value
	 * @param  array   $options
	 * @return string
	 *
	 * @throws \RuntimeException
	 */
	public function make($value, array $options = array())
	{
		$cost = isset($options['rounds']) ? $options['rounds'] : $this->rounds;
		$hash = password_hash($value, PASSWORD_BCRYPT, array('cost' => $cost));
		if ($hash === false)
		{
			throw new RuntimeException("Bcrypt hashing not supported.");
		}
		return $hash;
	}
	/**
	 * Check the given plain value against a hash.
	 *
	 * @param  string  $value
	 * @param  string  $hashedValue
	 * @param  array   $options
	 * @return bool
	 */
	public function check($value, $hashedValue, array $options = array())
	{
		return password_verify($value, $hashedValue);
	}
	/**
	 * Check if the given hash has been hashed using the given options.
	 *
	 * @param  string  $hashedValue
	 * @param  array   $options
	 * @return bool
	 */
	public function needsRehash($hashedValue, array $options = array())
	{
		$cost = isset($options['rounds']) ? $options['rounds'] : $this->rounds;
		return password_needs_rehash($hashedValue, PASSWORD_BCRYPT, array('cost' => $cost));
	}
	/**
	 * Set the default password work factor.
	 *
	 * @param  int  $rounds
	 * @return $this
	 */
	public function setRounds($rounds)
	{
		$this->rounds = (int) $rounds;
		return $this;
	}
}
```
除了实现 Contracts 中定义的三个方法外，还实现了一个额外 setRounds() 方法。 

下一步就是注册为服务提供者了： Illuminate/Hashing/HashServiceProvider.php 

```php
<?php namespace Illuminate\Hashing;
use Illuminate\Support\ServiceProvider;
class HashServiceProvider extends ServiceProvider {
	/**
	 * Indicates if loading of the provider is deferred.
	 *
	 * @var bool
	 */
	protected $defer = true;
	/**
	 * Register the service provider.
	 *
	 * @return void
	 */
	public function register()
	{
		$this->app->singleton('hash', function() { return new BcryptHasher; });
	}
	/**
	 * Get the services provided by the provider.
	 *
	 * @return array
	 */
	public function provides()
	{
		return array('hash');
	}
}
```
很明显的 register 了服务，具体做的就是 new 了一个 Container。

如果容器有多种实现，这时建议使用 Config 来配置服务了。

```php
$this->app->singleton('hash', function($app) { return new Hasher($app['config']['hash']); });
```
这时候 Hasher 实际上一个工厂类（Factory），使用配置来确定具体的，如 new Hasher('bcrypt') 来实例化。

如果服务要延迟载入，也就是按需载入。需要有一个激活标志，也就是 provides() 方法。 

当然， HashServiceProvider.php 必须在 config/app.php 的 providers 中定义 'Illuminate\Hashing\HashServiceProvider'。
 
这时，就可以使用：

```php
$this->app->make('hash')->make('password');
$this->app['hash']->make('password');
```
最后，声明一个 Facade： Illuminate/Support/Facades/Hash.php 

```php
<?php namespace Illuminate\Support\Facades;
/**
 * @see \Illuminate\Hashing\BcryptHasher
 */
class Hash extends Facade {
	/**
	 * Get the registered name of the component.
	 *
	 * @return string
	 */
	protected static function getFacadeAccessor()
	{
		return 'hash';
	}
}
```
在 config/app.php 的 aliases 中定义 'Hash' => 'Illuminate\Support\Facades\Hash'。 
那么，就不用上面长长的 $this->app->make('hash') ，直接：

```php
Hash::make('password');
```