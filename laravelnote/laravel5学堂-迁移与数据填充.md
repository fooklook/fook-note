##laravel5学堂-迁移与数据填充
laravel文档中写的不是很详细，在这里进行补充。
###表操作
在laravel中，表的创建、修改和删除是可以直接通代码来控制。有好处也有坏处。

好处：在代码迁移时，方便快捷。多人协作时，不需要沟通，直接通过代码就可以看懂。
坏处：很多数据库原生的表与表之间的关系，在这里很难直观提现出来。

####创建表
1. 命令创建表文件

```shell
//创建一个创建tablename表的迁移文件
php artisan make:migration create_tablename_table --create=tablename
//创建一个修改tablename表的迁移文件
php artisan make:migration add_column_tablename_table --table=tablename
```
执行后，将在database/migrations文件下多出一个data-tablename的文件。

2. 编辑表结构

在up中添加对应的代码创建对应的字段。

```php
命令 						功能描述
$table->bigIncrements('id'); 	ID 自动增量，使用相当于「big integer」类型
$table->bigInteger('votes'); 	相当于 BIGINT 类型
$table->binary('data'); 	相当于 BLOB 类型
$table->boolean('confirmed'); 	相当于 BOOLEAN 类型
$table->char('name', 4); 	相当于 CHAR 类型，并带有长度
$table->date('created_at'); 	相当于 DATE 类型
$table->dateTime('created_at'); 	相当于 DATETIME 类型
$table->decimal('amount', 5, 2); 	相当于 DECIMAL 类型，并带有精度与基数
$table->double('column', 15, 8); 	相当于 DOUBLE 类型，总共有 15 位数，在小数点后面有 8 位数
$table->enum('choices', array('foo', 'bar')); 	相当于 ENUM 类型
$table->float('amount'); 	相当于 FLOAT 类型
$table->increments('id'); 	相当于 Incrementing 类型 (数据表主键)
$table->integer('votes'); 	相当于 INTEGER 类型
$table->json('options'); 	相当于 JSON 类型
$table->jsonb('options'); 	JSONB equivalent to the table
$table->longText('description'); 	相当于 LONGTEXT 类型
$table->mediumInteger('numbers'); 	相当于 MEDIUMINT 类型
$table->mediumText('description'); 	相当于 MEDIUMTEXT 类型
$table->morphs('taggable'); 	加入整数 taggable_id 与字串 taggable_type
$table->nullableTimestamps(); 	与 timestamps() 相同，但允许 NULL
$table->smallInteger('votes'); 	相当于 SMALLINT 类型
$table->tinyInteger('numbers'); 	相当于 TINYINT 类型
$table->softDeletes(); 	加入 deleted_at 字段于软删除使用
$table->string('email'); 	相当于 VARCHAR 类型
$table->string('name', 100); 	相当于 VARCHAR 类型，并指定长度
$table->text('description'); 	相当于 TEXT 类型
$table->time('sunrise'); 	相当于 TIME 类型
$table->timestamp('added_on'); 	相当于 TIMESTAMP 类型
$table->timestamps(); 	加入 created_at 和 updated_at 字段
$table->rememberToken(); 	加入 remember_token 使用 VARCHAR(100) NULL
->nullable() 	标示此字段允许 NULL
->default($value) 	声明此字段的默认值
->unsigned() 	配置整数是无分正负
//加入索引
$table->string('email')->unique();
$table->primary('id');
$table->primary(array('first', 'last'));加入复合键
$table->unique('email');
$table->index('state');
//移除索引
$table->dropPrimary('users_id_primary');
$table->dropUnique('users_email_unique');
$table->dropIndex('geo_state_index');
//外键
$table->integer('user_id')->unsigned();
$table->foreign('user_id')->references('id')->on('users');
//你也可以指定选择在「on delete」和「on update」进行约束动作
$table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('cascade');
//移除外键
$table->dropForeign('posts_user_id_foreign');
```php

###修改或者删除表
```php
//修改表名
Schema::rename($from, $to);
//删除表
Schema::drop('users');
Schema::dropIfExists('users');
//修改字段属性
$table->string('name', 50)->nullable()->change();
//修改字段名称
$table->renameColumn('from', 'to');
//移除字段
$table->dropColumn('votes');
$table->dropColumn(['votes', 'avatar', 'location']);
//检测字段是否存在
if (Schema::hasTable('users')){}
if (Schema::hasColumn('users', 'email')){}
//移除时间戳记和软删除
$table->dropTimestamps();	//移除 created_at 和 updated_at 字段
$table->dropSoftDeletes();	//移除 deleted_at 字段
//修改表索引
Schema::create('users', function($table)
{
    $table->engine = 'InnoDB';

    $table->string('email');
});
```
###数据填充
首先创建seeder文件

```shell
//laravel5.1版本以上支持
php artisan make:seeder tablenameSeeder
```
执行命令后，将会在atabase/seeds目录下创建一个文件。
也可以手动创建。

```php
<?php
use Illuminate\Database\Seeder;
use \App\User;
class AdminUserSeeder extends Seeder {
    public function run(){
	//在这里填写你想要填充的数据库
	DB::table('users')->truncate();//清空表数据
	$user = new User();
	$user->name = "admin";
	$user->save();
    }
}
```

配置DatabaseSeeder

```php
public function run()
{
	Model::unguard();
	$this->call('UserTableSeeder');
}
```

执行填充数据

```shell
php artisan db:seed
//执行单个文件
php artisan db:seed --class=UserTableSeeder
```