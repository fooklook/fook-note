##jadenode

###安装jade

windows环境下，安装nodejs后，创建node-test文件夹。

进入node-test文件夹后，执行npm install jade -g命令即可。

###编译jade

jade index.jade index.html

```node
jade index.jade index.html -P       //生成不压缩的文件
jade index.jade index.html -w       //监听index.jade,如果文件发生修改，就编译。
```


###基本标签语法

```jade
doctype html
html
  head
    meta(charset="utf-8")/
    - var title="标题"
    title title
  body
    h1 demo
    ul
     li:a(href="http://www.fooklook.com")
     li: a(href="http://www.fooklook.com"): img(src="http://fooklook.com/logo.png")/
   p#name test
   p.name test
   p(style={color:'#010101',width:'100px'})
   p(style="color:#010101;width:100px")
   
   form(action="/login" )
      input(name="name" value="1")
      input(name="name", value="1")
      
    |纯文本
    
    //输出注释
    //-不输出注释
    //
      输出注释
      再次注释
```

###语法

```jade
doctype html
html
  head
    meta(charset="utf-8")/
    title 标题
  body
    ul
      each name in [1,2,3]
        li=name
    ul
      each val, index in ['zero', 'one', 'two']
        li= index + ': ' + val
    ul
      each val, index in {1:'one',2:'two',3:'three'}
        li= index + ': ' + val
    - var desc = true
    if desc
      p.desc=desc
    case num
      when 9
        p.name=9
      when 10
        p.name=num
      default
        p#name=num
    - var authenticated = true
    div(class=authenticated ? 'authed' : 'anon')
```

###非转译属性

```jade
div(escaped="<code>")
div(unescaped!="<code>")
```

生成

```html
<div escaped="&lt;code&gt;"></div>
<div unescaped="<code>"></div>
```

###自定义函数

```jade
mixin pet(name)
  li.pet= name
ul
  +pet('cat')
  +pet('dog')
  +pet('pig')
```

生成

```html
<ul>
  <li class="pet">cat</li>
  <li class="pet">dog</li>
  <li class="pet">pig</li>
</ul>
```

####块

```jade
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p No content provided

+article('Hello world')

+article('Hello world')
  p This is my
  p Amazing article
```

生成

```html
<div class="article">
  <div class="article-wrapper">
    <h1>Hello world</h1>
    <p>No content provided</p>
  </div>
</div>
<div class="article">
  <div class="article-wrapper">
    <h1>Hello world</h1>
    <p>This is my</p>
    <p>Amazing article</p>
  </div>
</div>
```

####属性

```jade
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class, href=href)= name
+link('/foo', 'foo')(class="btn")

input(type='checkbox', checked)
input(type='checkbox', checked=true)
input(type='checkbox', checked=false)
input(type='checkbox', checked=true.toString())
```

生成

```html
<a href="/foo" class="btn">foo</a>

<input type="checkbox" checked="checked"/>
<input type="checkbox" checked="checked"/>
<input type="checkbox"/>
<input type="checkbox" checked="true"/>
```

####剩余参数

```jade
mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item

+list('my-list', 1, 2, 3, 4)
```

生成

```html
<ul id="my-list">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ul>
```

###继承

```jade
//- layout.jade
doctype html
html
  head
    block title
      title Default title
  body
    block content
//- index.jade
extends ./layout.jade

block title
  title Article Title

block content
  h1 My Article
```

生成

```html
<!doctype html>
<html>
  <head>
    <title>Article Title</title>
  </head>
  <body>
    <h1>My Article</h1>
  </body>
</html>
```

###包含

```jade
//- index.jade
doctype html
html
  include ./includes/head.jade
  body
    h1 My Site
    p Welcome to my super lame site.
    include ./includes/foot.jade
//- includes/head.jade
head
  title My Site
  script(src='/javascripts/jquery.js')
  script(src='/javascripts/app.js')
//- includes/foot.jade
#footer
  p Copyright (c) foobar
```

生成

```html
<!doctype html>
<html>
  <head>
    <title>My Site</title>
    <script src='/javascripts/jquery.js'></script>
    <script src='/javascripts/app.js'></script>
  </head>
  <body>
    <h1>My Site</h1>
    <p>Welcome to my super lame site.</p>
    <div id="footer">
      <p>Copyright (c) foobar</p>
    </div>
  </body>
</html>
```

###插入变量

```jade
- var title = "On Dogs: Man's Best Friend";
- var author = "enlore";
- var theGreat = "<span>escape!</span>";

h1= title
p Written with love by #{author}
p This will be safe: #{theGreat}

- var msg = "not my inside voice";
p This is #{msg.toUpperCase()}

- var riskyBusiness = "<em>Some of the girls are wearing my mother's clothing.</em>";
.quote
  p Joel: !{riskyBusiness}
```

生成

```html
<h1>On Dogs: Man's Best Friend</h1>
<p>Written with love by enlore</p>
<p>This will be safe: &lt;span&gt;escape!&lt;/span&gt;</p>
<p>This is NOT MY INSIDE VOICE</p>
<div class="quote">
  <p>Joel: <em>Some of the girls are wearing my mother's clothing.</em></p>
</div>
```