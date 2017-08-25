##laravel5学堂-常用技巧

###获取sql语句

```php
DB::enableQueryLog();
$user = App\User::all();
return response()->json(DB::getQueryLog());
```

###where中的()条件

```php
$count = DB::table('table_name')
->where("id", "<>", $id)
->where(
    function($query) use($request){
    $query->where("title","=",$request->title)
    ->orWhere("mini_title","=",$request->mini_title);
    }
)
->count();
//sql select count(*) from table_name where id<>1 and (title="title" and mini_title="mini_title");
```