##gulpnote
###Gulp工具能实现新的功能。

- 编译预处理CSS
- 压缩JavaScript、css
- 监听文件修改，自动刷新浏览器

###什么是Gulp
Gulp相当于网站开发的一种工具，它并不是网站开发语言，用来开发网站的相关功能。但是它能代替你执行常见的任务。比如，刷新浏览器，编译LESS文件。

Gulp是基于Node.js构建的，所以它的代码必须写在javascript文件中，你也可以使用coffeescript。

Gulp强大的功能主要来自它大量的插件。

Gulp比Grunt效率更高，而且代码更加简单。

###安装Gulp
安装Gulp前，你需要安装Node.js，windows用户只需要到[官网](https://nodejs.org/en/)下载安装就可以。其它系统的，直接通过命令行安装。

安装全局Gulp

```shell
//使用国内源，更新安装更快
npm install -g gulp --registry=http://registry.npm.taobao.org
```

在test项目中安装Gulp

```shell
//创建test文件
mkdir test
cd test
//安装Gulp，使用国内源，更新安装更快
npm install gulp --save-dev --registry=http://registry.npm.taobao.org
```
不过建议通过创建配置package.json文件安装。

```json
//package.json文件，gulp可以不指定版本，系统会安装最新版本。
{
  "private": true,
  "devDependencies": {
    "gulp": "^3.8.8"
  }
}
//执行安装命令
npm install
```
###Gulp使用
####通过Gulp时间压缩javascript文件。

首先在test项目的根目录中创建一个gulpfile.js文件。

```javascript
var gulp = require('gulp'),
   uglify = require('gulp-uglify');//这个就是压缩js文件的插件
gulp.task('minify', function () {
   gulp.src('js/*.js')
      .pipe(uglify())
      .pipe(gulp.dest('assets/js'));
});
```
解析一下上面的代码，通过var声明另个变量，引入gulp和gulp-uglify插件。
minify是通过运行 `gulp minify`，执行里面的函数。  
然后将js/*.js文件压缩到assets/js目录下对应的文件中。  

安装gulp-uglify插件

和安装gulp一样，有两种方法安装。

```shell
//在package.json文件中，添加插件
{
  "private": true,
  "devDependencies": {
    "gulp": "^3.8.8",
    "gulp-uglify": ""
  }
}
//执行更新命令
npm update
```

```
//直接执行安装命令
npm install -–save-dev gulp-uglify --registry=http://registry.npm.taobao.org
```
通过使用共同使用不同的插件，可以将多个文件压缩到同一个文件中。这样在部署网站的时候，因为js文件被压缩在同一个文件，使得网站的访问速度加快。

####监听文件修改，自动更新浏览器
查看了网上很多的案例，才总结了一下内容，确保一次成功。

传统的方法是通过livereload+chrome的livereload插件来实现，首先需要安装chrome的插件，这个就屏蔽了很多人。

但是通过以下方法，就可以做到，所有浏览器只要通过指定端口，就能做到实时刷新。

首先需要的gulp组件

- gulp-connect
- gulp-livereload 
- gulp-uglify	//压缩js文件
- gulp-minify-css //压缩css文件

gulpfile.js源码

```javascript
var gulp = require('gulp'),
   uglify = require('gulp-uglify');		//js压缩
   mincss = require("gulp-minify-css"),				//css压缩
   livereload = require('gulp-livereload'),
   connect = require('gulp-connect');
gulp.task('minify', function () {
   gulp.src('js/*.js')
      .pipe(uglify())
      .pipe(gulp.dest('assets/js'));
   gulp.src('css/*.css')
      .pipe(mincss())
      .pipe(gulp.dest('assets/css'))
      .pipe(connect.reload());
   gulp.src();  
   console.log('yes');
});
gulp.task('watch',function(){
    gulp.watch('js/*.js',['minify']);
});
gulp.task('reload',function(){
	connect.server({
		root: "./",
		port: 8000,
		livereload: true
	});
	gulp.watch(['css/*.css','js/*.js','./*.html'],['minify']);
});
```
以上的代码意思是，通过gulp-connect设置一个服务器，用来监听8000端口，通过watch监听所有文件的变化，用gulp-livereload来进行实时刷新。

在任何浏览器，输入localhost:8000就可以访问root目录下的指定文件。指定的文件发生修改，就会自动刷新页面。

常用插件介绍：

gulp-cache：图片缓存，只有图片替换了才压缩  
gulp-uglify：js文件压缩  
gulp-concat：文件合并  
gulp-minify-css：css文件压缩  
gulp-imagemin：图片压缩  
gulp-rename：文件重命名  
比如压缩后的css文件名添加min   
.pipe(rename({suffix: '.min'}))  
gulp-notify：消息通知  
gulp-autoprefixer：自动补全css前缀  
gulp-livereload：自动刷新  
gulp-connect：WEB测试服务器  
gulp-coffee：编译coffee代码为Js代码，使用coffeescript必备  
