##Gulp（部分转载）
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
      .pipe(gulp.dest('assets/js'))
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
http://www.imooc.com/article/2364