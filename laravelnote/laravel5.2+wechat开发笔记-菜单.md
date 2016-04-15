##laravel5.2+wechat开发笔记-菜单

###创建普通菜单

####创建menu路由

```php
Route::get('menu', 'WechatController@menu');
```

####创建控制器

```php
	//自定义菜单
    public function menu(Application $wechat){
        $menu = $wechat->menu;
        $buttons = [
            [
                "type" => "click",
                "name" => "今日歌曲",
                "key"  => "V1001_TODAY_MUSIC"
            ],
            [
                "name"       => "菜单",
                "sub_button" => [
                    [
                        "type" => "view",
                        "name" => "搜索",
                        "url"  => "http://www.soso.com/"
                    ],
                    [
                        "type" => "view",
                        "name" => "视频",
                        "url"  => "http://v.qq.com/"
                    ],
                    [
                        "type" => "click",
                        "name" => "赞一下我们",
                        "key" => "V1001_GOOD"
                    ],
                ],
            ],
        ];
        $menu->add($buttons);
    }
```

###创建个性化菜单

微信公众号可以为不同用户设定不同的菜单。不过在创建菜单时，要多加一些条件，比如用户管理组、性别和地区。

微信为了区分不同的菜单，会为提交的个性化菜单设定一个menuid，在查询菜单时将会返回。

####提交个性化菜单

```php
	public function menu(Application $wechat){
        $menu = $wechat->menu;
        $buttons = [
            [
                "type" => "click",
                "name" => "今日歌曲",
                "key"  => "V1001_TODAY_MUSIC"
            ],
            [
                "name"       => "菜单",
                "sub_button" => [
                    [
                        "type" => "view",
                        "name" => "搜索",
                        "url"  => "http://www.soso.com/"
                    ],
                    [
                        "type" => "view",
                        "name" => "视频",
                        "url"  => "http://v.qq.com/"
                    ],
                    [
                        "type" => "click",
                        "name" => "赞一下我们",
                        "key" => "V1001_GOOD"
                    ],
                ],
            ]
        ];
        $matchRule = [
            "group_id"             => "2",
            "sex"                  => "1",
            "country"              => "中国",
            "province"             => "广东",
            "city"                 => "广州",
            "client_platform_type" => "2"
        ];
        $menu->add($buttons,$matchRule);
```

```php
//查询所有菜单
$menus = $menu->all();
//查询自定义菜单。菜单功能可以通过微信公众账号管理平台和调用API两种方式设置菜单。通过该方式，可以获取通过公众平台设置的菜单内容。
$menus = $menu->current();
//删除全部菜单
$menu->destroy();
//单独删除自定义菜单
$menu->destroy($menuId);
//测试个性化菜单
$menus = $menu->test($userId);
```

