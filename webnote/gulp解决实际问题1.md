##gulp解决实际问题1

###解决一下问题

- css、js和图片的版本问题
- 图片上传到CDN(七牛)服务器
- 文件管理问题

###代码

```js
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),//压缩js文件
    mincss = require("gulp-minify-css"), //压缩html文件
    qiniu = require('gulp-qiniu'),//上传七牛
    imagemin = require('gulp-imagemin'),//图片压缩
    rev = require('gulp-rev'),
    clean = require('gulp-clean'),
    revCollector = require("gulp-rev-collector"),
    runSequence = require('run-sequence');


filename = "test20160607/";
qiniuURL = 'http://7xp3vm.com1.z0.glb.clouddn.com';
//---------------------------------------------
//-----------------清除旧文件------------------
gulp.task('clean', function() {
	gulp.src('dist', {read: false})
	.pipe(clean());

	gulp.src('rev', {read: false})
	.pipe(clean());
	return true;
});

//---------------------------------------------
//---------------修改图片版本号----------------
gulp.task('image', function () {
	return gulp.src("./image/*.*")
	.pipe(rev())
	.pipe(gulp.dest('dist/image'))
	.pipe(rev.manifest())
	.pipe(gulp.dest('rev/image/'));
});

//---------------------------------------------
//---------------上传到七牛服务器--------------
gulp.task('qiniu', ['image'], function(){
	gulp.src("./dist/image/*.*")
	.pipe(
		qiniu({
		accessKey: "_l6ou70rito3ZHbMjJ5aoIy3BE2V33rbHjRVX2EN",
		secretKey: "QasDgr3mCMI3JQTrSXOekgxTJwpVH0O1bd62_xBt",
		bucket: "fooklook",
		private: false
		}, {
		dir: filename,
		versioning: false,
		versionFile: 'rev/image/qiniucdn.json',
		concurrent: 10
		})
	);
});

//---------------------------------------------
//------修改css版本号，并上传到七牛服务器------
gulp.task("css", ['qiniu'], function(){
	return gulp.src(['./rev/image/*.json', "./css/*.css"])
	.pipe(revCollector({
		replaceReved: true,
		dirReplacements: {
			'cdn/image': function(manifest_value) {
				return qiniuURL + '/' + filename + manifest_value;
			}
		}
	}))
	.pipe(mincss())
	.pipe(rev())
	.pipe(gulp.dest('dist/css'))
	.pipe(rev.manifest())
	.pipe(gulp.dest('rev/css'));
})

//---------------------------------------------
//------修改js版本号，并上传到七牛服务器-------
gulp.task("js", ['qiniu'], function(){
	return gulp.src(['./rev/image/*.json', "./js/*.js"])
	.pipe(revCollector({
		replaceReved: true,
		dirReplacements: {
			'cdn/image': function(manifest_value) {
				return qiniuURL + '/' + filename + manifest_value;
			}
		}
	}))
	.pipe(uglify())
	.pipe(rev())
	.pipe(gulp.dest('dist/js'))
	.pipe(rev.manifest())
	.pipe(gulp.dest('rev/js'));
})

//---------------------------------------------
//------修改html文件中替换对应的地址
gulp.task("html" , ['css', 'js'],function(){
	//替换html代码中图片的路径
	return gulp.src(['./rev/**/*.json', './html/*.html'])
	.pipe(revCollector({
		replaceReved: true,
		dirReplacements: {
			'../css': function(manifest_value){
				return './css/' + manifest_value;
			},
			'../js': function(manifest_value){
				return './js/' + manifest_value;
			},
			'cdn/image': function(manifest_value) {
				return qiniuURL + '/' + filename + manifest_value;
			}
		}
	}))
	.pipe(gulp.dest('dist'));
});

gulp.task('runing', ['html'], function(){

});
```