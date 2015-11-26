##laravel5学堂-视图
视图对于使用过框架的开发者来说，是绝对熟悉的。但在我看来，如果你的大量工作都是在简单的调取数据库数据，然后套页面的话。这样和前端的**切片仔**没什么区别，重复流水线上的工作。对于重新**嵌套**来说，可能并不难，但是如果维护话。那就相当的麻烦了。所以请慎重对待嵌套这份工作，并且使用laravel的一些**组件**，你会佩服自己的机智，和编程这份工作的伟大，把简单枯燥的工作，干成自己的理想。
###视图文件存储路径、文件命名及调用视图方式
在laravel中，没有创建视图文件的命令，可能框架设计者认为这些工作都应该由前端开发人员来完成。

首先视图文件全部放在资源文件夹resources/view文件中，同时所有的css、js和图片文件放在assets文件中，在视图中，通过asset函数，生成文件路径。

视图的文件的统一命名方式为：path\viewname.blade.php
调用视图获取的方式是 `path.view`。

###向视图传递参数

```php
//在路由中电泳视图并传递参数
Route::get('/', function()
{
    return view('viewname', array('name' => 'James');
});
```
在这里建议大家通过array()的方式声明数组。在js语法中也通过new array()来声明变量。

简便方法，向视图传值。compact()函数

```php
$firstname = "Peter";
$lastname = "Griffin";
$age = "38";
$result = compact("firstname", "lastname", "age");
return view('viewname', $result);
```
###基本嵌套语法

```php
//php正式语法
 <h1>hello <?php echo $name; ?></h1>
//Blade语法结构
//{{ }}双大括号。打印数据
{{ $name }}//打印出的数据，例如html代码，将被转义。
//{!!  !!}打印数据
{!! $name !!}//和{{  }}类似，但是不转义字符。
```

###嵌套语法
####if语句

```php
//简单判断
@unless (Auth::check())
    You are not signed in.
@endunless
//正式语法
@if ()
@elseif ()
@else
@endif
```
####循环

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```
####其他语法
解析相关概念

主题模板：一个网站的header和footer一般情况下是一样的，避免重复代码，所以通过使用主题模板，将共同部分放在同一个文件中，然后其它页面对其进行调用。

代码注释

```php
{{-- This comment will not be in the rendered HTML --}}
```
模板

```php
//模板区块填充主题模板声明
@yield('title')
@yield('content');
@yield('default', 'default');//为区块设置默认值

//调用主题模板
@extends('layouts.master')

//模板区块  一个主题模板中，可以包含多个模块。
@section('title','hello')//直接赋值
@section('content')
//content
@endsection

//加载子视图
@include('view.name')
//向子视图传值
@include('view.name', ['value' => $value]);

//对于区块重写时使用@@parent,继承父辈区块内容
@section('sidebar')
parent
@endsection
@section('sidebar')
@@parent
children
@endsection 
//将同时显示两父辈区块内容和继承区块内容

//显示lang文件中的语言行
@lang('language.line')
@choice('language.line', 1)
```