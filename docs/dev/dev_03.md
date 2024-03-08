# 部分 I. Web page

## 第 4 章 gulpjs

安装

```
npm install gulp-cli
npm install gulp -D

```

创建 gulpfile.js 文件

```

var gulp = require('gulp');
var pug = require('gulp-pug');
var less = require('gulp-less');
var minifyCSS = require('gulp-csso');

gulp.task('html', function(){
  return gulp.src('client/templates/*.pug')
    .pipe(pug())
    .pipe(gulp.dest('build/html'))
});

gulp.task('css', function(){
  return gulp.src('client/templates/*.less')
    .pipe(less())
    .pipe(minifyCSS())
    .pipe(gulp.dest('build/css'))
});

gulp.task('default', [ 'html', 'css' ]);

```

排除目录

```

gulp.src(['js/**/*.js', '!js/**/*.min.js'])

```

## Tasks automation

### gulp-changed

```

// npm install --save-dev gulp gulp-changed gulp-jscs gulp-uglify

var gulp = require('gulp');
var changed = require('gulp-changed');
var jscs = require('gulp-jscs');
var uglify = require('gulp-uglify');

var SRC = 'src/**/*.js';
var DEST = 'dist';

gulp.task('default', function() {
    return gulp.src(SRC)
        .pipe(changed(DEST)) // changed 任务需要提前知道目标目录位置才能找出哪些文件是被修改过的,只有被更改过的文件才会通过这里
        .pipe(jscs())
        .pipe(uglify())
        .pipe(gulp.dest(DEST));
});	

```

### 显示处理进度

显示处理中的文件

```

gulp.task('minify-css', function () {

        gulp.src([src + '/**/css/**/*.css', "!"+src + '/**/css/**/*.min.css'])
        .on('data', function(file) {
                console.log('%s Read %d bytes of data', file.path, file.contents.length);
        })
        .pipe(concat("finally.css"))
        .pipe(rename({ suffix: '.min' }))
        .pipe(minifycss())
        .pipe(gulp.dest( dist ));

});	

```

### notify

```

npm install --save-dev gulp-notify

```

```

var notify = require('gulp-notify');

```

```

.pipe(notify({ message: 'Styles task complete' }));

```

### del

```

var gulp = require('gulp');
var del = require('del');

gulp.task('clean:mobile', function (cb) {
  del([
    'dist/**/css/*.min.css',
    'dist/mobile/**/*',
    '!dist/mobile/deploy.json'
  ], cb);
});

gulp.task('default', ['clean:mobile']);			

// Clean
gulp.task('clean', function() {
	return del(['dist/styles', 'dist/scripts', 'dist/images']);
});

```

### start

```

// Default task
gulp.task('default', ['clean'], function() {
	gulp.start('styles', 'scripts', 'images');
});

```

## watch

```

// including plugins
var gulp = require('gulp');
var watch = require('gulp-watch');

gulp.task('watch', function() {
    watch(__dirname + "/css/**/*.css", function() {
        gulp.run('minify-css');
    });
    watch(__dirname + "/js/**/*.js", function() {
        gulp.run('minify-js');
    });
});		

```

```

gulp watch		

```

## HTML Minification

```

npm install --save-dev gulp-minify-html

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var minifyHtml = require("gulp-minify-html");

// task
gulp.task('minify-html', function () {
    gulp.src('./html/*.html') // path to your files
    .pipe(minifyHtml())
    .pipe(gulp.dest('path/to/destination'));
});		

```

```

gulp minify-html

```

禁止处理 SSI 服务器端包含

```

gulp.task('minify-html', function () {
    gulp.src( src +'/cfd/*.html')
    .pipe(minifyHtml({ssi:true, quotes:false}))
    .pipe(gulp.dest( dist + '/cfd/'));
});

```

## CSS Minification

### gulp-minify-css

```

npm install --save-dev gulp-minify-css

```

```

// including plugins
var gulp = require('gulp');
var minifyCss = require("gulp-minify-css");

// task
gulp.task('minify-css', function () {
    gulp.src('./css/one.css') // path to your file
    .pipe(minifyCss())
    .pipe(gulp.dest('path/to/destination'));
});

// task
gulp.task('minify-multi-css', function () {
    gulp.src(__dirname+'/css/*.css') // path to your file
    .pipe(minifyCss())
    .pipe(gulp.dest('path/to/destination'));
});

```

```

gulp minify-css
gulp minify-multi-css

```

### gulp-clean-css

```

var gulp = require('gulp');
var minifycss = require('gulp-clean-css');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

gulp.task('minify-css', function () {

        gulp.src('./css/*.css')
        .pipe(concat("finally.css"))
        .pipe(rename({ suffix: '.min' }))
        .pipe(minifycss())
        .pipe(gulp.dest('build/css'));

});

gulp.task('minify-js', function(){

        gulp.src(__dirname + "/js/*.js")
        .pipe(concat("finally.js"))
        .pipe(rename({ suffix: '.min' }))
        .pipe(uglify())
        .pipe(gulp.dest('build/js'))

});

```

compatibility

```

var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');

gulp.task('minify-css', function() {
  return gulp.src('styles/*.css')
    .pipe(cleanCSS({compatibility: 'ie8'}))
    .pipe(gulp.dest('dist'));
});			

```

callback

```

var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');

gulp.task('minify-css', function() {
    return gulp.src('styles/*.css')
        .pipe(cleanCSS({debug: true}, function(details) {
            console.log(details.name + ': ' + details.stats.originalSize);
            console.log(details.name + ': ' + details.stats.minifiedSize);
        }))
        .pipe(gulp.dest('dist'));
});			

```

### gulp-make-css-url-version

给 css 文件里引用 url 加版本号（md5sum），像这样：

```

background: url(../images/pc-banner-bg.jpg?v=4facbd0914639f296faec4dba4d358f0) no-repeat;}

```

```

npm install --save-dev gulp-make-css-url-version 

```

```

var gulp = require('gulp'),
minifier = require('gulp-minify-css');
cssver = require('gulp-make-css-url-version'); 

gulp.task('testCssmin', function () {
    gulp.src('src/css/*.css')
        .pipe(cssver()) //给 css 文件里引用文件加版本号（文件 MD5）
        .pipe(minifier())
        .pipe(gulp.dest('dist/css'));
});			

```

### CSS 冗余分析

检查出重复定义的 CSS

Installation

```

npm install gulp-csscss --save-dev			

```

Example

```

var gulp = require('gulp');
var csscss = require('gulp-csscss');

gulp.task('default', function() {
  gulp.src('src/style.css')
    .pipe(csscss())
});

```

## JS Minification

```

npm install --save-dev gulp-uglify

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var uglify = require("gulp-uglify");

// task
gulp.task('minify-js', function () {
    gulp.src('./JavaScript/*.js') // path to your files
    .pipe(uglify())
    .pipe(gulp.dest('path/to/destination'));
});

```

Run:

```

gulp minify-js

```

### JS 校验

```

var gulp = require('gulp');
var header = require('gulp-header');  //给文本文件头部追加内容
var footer = require('gulp-footer');
var concat = require('gulp-concat');
var jshint = require('gulp-jshint'); //js 代码校验
var cached = require('gulp-cached');
var remember = require('gulp-remember'); //gulp-remember is a gulp plugin that remembers files that have passed through it. gulp-remember adds all the files it has ever seen back into the stream.

gulp.task('scripts', function() {
  return gulp.src('src/**/*.js')
      .pipe(cached('scripts'))        // 只传递更改过的文件 
      .pipe(jshint())                 // 对这些更改过的文件做一些特殊的处理...
      .pipe(header('(function () {')) // 比如 jshinting ^^^
      .pipe(footer('})();'))          // 增加一些类似模块封装的东西
      .pipe(remember('scripts'))      // 把所有的文件放回 stream
      .pipe(concat('main.js'))         // 合并文件的操作
      .pipe(gulp.dest('public/'));
});

```

## CSS Sprite

```

简介
gulp-spriter：帮助前端工程师将 css 代码中的切片图片合并成雪碧图，支持 retina 图片。

功能
使用二叉树排列算法，对图片排序优化
自动收集 css 中带切片的图片（仅对 background-image:url("slice/xx.png")有效）
自动在原来的 css 中添加 background-position 属性
支持生成适用于高清设备的雪碧图，并在 css 文件追加媒体查询 css 代码
依赖
gulp-spriter 使用 spritesmith 作为图片生成的基础算法

安装
npm install gulp-spriter
配置
导入 gulp-spriter 依赖：

var spriter = require("gulp-spriter");

gulpfile 配置文件中增加 task，如下：

gulp.task("css",["clean"],function(){
  return gulp.src("./src/css/xxx.css")
         .pipe(spriter({
            sprite:"test.png",
            slice:"./src/slice",
            outpath:"./build/tests"
          }))
         .pipe(gulp.dest('./build/css'))
})
参数
sprite:[string] 必须，设置输出的雪碧图名称
slice：[string] 必须，切片文件存放位置，基于根目录
outpath：[string] 必须，输出的雪碧图位置

```

## Compress Images

```

optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
progressive: true, //类型：Boolean 默认：false 无损压缩 jpg 图片
interlaced: true, //类型：Boolean 默认：false 隔行扫描 gif 进行渲染
multipass: true //类型：Boolean 默认：false 多次优化 svg 直到完全优化

```

```

var imagemin = require('gulp-imagemin');

gulp.task('images', function() {
  return gulp.src('src/images/**/*')
    .pipe(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true }))
    .pipe(gulp.dest('dist/assets/img'))
    .pipe(notify({ message: 'Images task complete' }));
});		

```

## WEBP 格式图片

```

npm install --global gulp-imageisux
npm install --save-dev gulp-imageisux

var imageisux = require('gulp-imageisux');

gulp.task('imageisux', function() {
	return gulp.src(['img/*'])
			   .pipe(imageisux('/dirpath/',true));
});

```

## Sass Compilation

Using gulp-sass

```

npm install --save-dev gulp-sass

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var sass = require("gulp-sass");

// task
gulp.task('compile-sass', function () {
    gulp.src('./Sass/one.sass') // path to your file
    .pipe(sass())
    .pipe(gulp.dest('path/to/destination'));
});

```

Run:

```

gulp compile-sass

```

## Less Compilation

Using gulp-less

```

npm install --save-dev gulp-less

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var less = require("gulp-less");

// task
gulp.task('compile-less', function () {
    gulp.src('./Less/one.less') // path to your file
    .pipe(less())
    .pipe(gulp.dest('path/to/destination'));
});	

```

Run:

```

gulp compile-less

```

## 重命名文件名

```

Using gulp-rename

npm install --save-dev gulp-rename

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var rename = require('gulp-rename');
var coffee = require("gulp-coffee");

// task
gulp.task('rename', function () {
    gulp.src('./CoffeeScript/one.coffee') // path to your file
    .pipe(coffee())  // compile coffeeScript
    .pipe(rename('renamed.js')) // rename into "renamed.js" (original name "one.js")
    .pipe(gulp.dest('path/to/destination'));
});

```

Run:

```

gulp rename

```

## 合并文件

Concatenate files using gulp-concat

```

npm install --save-dev gulp-concat		

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp');
var concat = require("gulp-concat");

// task
gulp.task('concat', function () {
    gulp.src('./javascript/*.js') // path to your files
    .pipe(concat('concat.js'))  // concat and name it "concat.js"
    .pipe(gulp.dest('path/to/destination'));
});

```

Run:

```

gulp concat			

```

## 文件头

```

Using gulp-header and Node’s file system

npm install --save-dev gulp-header		

```

Copyright 头文件

```

# vim Copyright

/*
Author: netkiller <netkiller@msn.com>
Website: https://www.netkiller.cn
Version: <%= version %>
*/

```

Version 文件

```

# vim Version
1.0.0

```

gulpfile.js:

```

// including plugins
var gulp = require('gulp')
, fs = require('fs')
, concat = require("gulp-concat")
, header = require("gulp-header");

// functions

// Get version using NodeJs file system
var getVersion = function () {
    return fs.readFileSync('Version');
};

// Get copyright using NodeJs file system
var getCopyright = function () {
    return fs.readFileSync('Copyright');
};

// task
gulp.task('concat-copyright-version', function () {
    gulp.src('./javascript/*.js')
    .pipe(concat('finaly.js')) // concat and name it "concat-copyright-version.js"
    .pipe(header(getCopyrightVersion(), {version: getVersion()}))
    .pipe(gulp.dest('path/to/destination'));
});

```

Run:

```

gulp concat-copyright-version			

```

## yargs 命令行参数传递

```

npm install --save-dev yargs

```

```

var argv = require('yargs').argv;

gulp.task('my-task', function() {
    return gulp.src(argv.a == 1 ? options.SCSS_SOURCE : options.OTHER_SOURCE)
        .pipe(sass({style:'nested'}))
        .pipe(autoprefixer('last 10 version'))
        .pipe(concat('style.css'))
        .pipe(gulp.dest(options.SCSS_DEST));
});		

```

```

var argv = require('yargs').argv,
    gulpif = require('gulp-if'),
    rename = require('gulp-rename'),
    uglify = require('gulp-uglify');

gulp.task('my-task-stage', function() {
  gulp.src('src/**/*.js')
    .pipe(concat('out.js'))
    .pipe(gulpif(argv.production, uglify()))
    .pipe(gulpif(argv.production, rename({suffix: '.min'})))
    .pipe(gulp.dest('dist/'));
});		

```

```

gulp my-task -a 1
gulp my-task-stage --production

```

### gulp-util

```

var util = require('gulp-util');

gulp.task('styles', function() {
  return gulp.src(['src/styles/' + (util.env.theme ? util.env.theme : 'main') + '.scss'])
    .pipe(compass({
        config_file: './config.rb',
        sass   : 'src/styles',
        css    : 'dist/styles',
        style  : 'expanded'

    }))
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'ff 17', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(livereload(server))
    .pipe(gulp.dest('dist/styles'))
    .pipe(notify({ message: 'Styles task complete' }));
});		

```

```
gulp watch --theme literature

```

### minimist

```

var gulp   = require('gulp');

// npm install gulp yargs gulp-if gulp-uglify
var args   = require('yargs').argv;
var gulpif = require('gulp-if');
var uglify = require('gulp-uglify');

var isProduction = args.env === 'production';

gulp.task('scripts', function() {
  return gulp.src('**/*.js')
    .pipe(gulpif(isProduction, uglify())) // only minify if production
    .pipe(gulp.dest('dist'));
});

gulp scripts --env production

```

```

Pass arguments from the command line

// npm install --save-dev gulp gulp-if gulp-uglify minimist

var gulp = require('gulp');
var gulpif = require('gulp-if');
var uglify = require('gulp-uglify');

var minimist = require('minimist');

var knownOptions = {
  string: 'env',
  default: { env: process.env.NODE_ENV || 'production' }
};

var options = minimist(process.argv.slice(2), knownOptions);

gulp.task('scripts', function() {
  return gulp.src('**/*.js')
    .pipe(gulpif(options.env === 'production', uglify())) // only minify in production
    .pipe(gulp.dest('dist'));
});
Then run gulp with:

$ gulp scripts --env development

```

## gulp-sourcemaps

```

var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});	

```

## gulp-zip

```

var gulp             = require( 'gulp' ),
    zip              = require( 'gulp-zip' );

gulp.task( 'zip', function() {

  return gulp.src( [
    '!{.gitignore,README.md}',
    '!node_modules', '!node_modules/**',
    '!dist', '!dist/**',
    './**',
  ] )
    .pipe( zip('archive.zip') )
    .pipe( gulp.dest( 'dist' ) );

});	

```

## 清理 JS 中的 console.log()调试语句

```

var gulp = require('gulp');
var stripDebug = require('gulp-strip-debug');

gulp.task('default', function () {
    return gulp.src('src/app.js')
        .pipe(stripDebug())
        .pipe(gulp.dest('dist'));
});

```

## copy-dir

install

```

npm install copy-dir		

```

usage

```

Sync:

var copydir = require('copy-dir');

copydir.sync('/my/from/path', '/my/target/path');
Async:

var copydir = require('copy-dir');

copydir('/my/from/path', '/my/target/path', function(err){
  if(err){
    console.log(err);
  } else {
    console.log('ok');
  }
});
add a filter
When you want to copy a directory, but some file or sub directory is not you want, you can do like this:

Sync:

var path = require('path');
var copydir = require('copy-dir');

copydir.sync('/my/from/path', '/my/target/path', function(stat, filepath, filename){
  if(stat === 'file' && path.extname(filepath) === '.html') {
    return false;
  }
  if (stat === 'directory' && filename === '.svn') {
    return false;
  }
  return true;
}, function(err){
  console.log('ok');
});
Async:

var path = require('path');
var copydir = require('copy-dir');

copydir('/a/b/c', '/a/b/e', function(stat, filepath, filename){
  //... 
}, function(err) {
  //... 
});

```

## gulp-copy

Usage

```

// gulpfile.js

var gulpCopy = require('gulp-copy');
var sourceFiles = [ 'source1/*', 'source2/*.txt' ];
var destination = 'dest/';

return gulp
    .src(sourceFiles)
    .pipe(gulpCopy(outputPath, options))
    .dest(destination);

```

## Example

### HTML,JS,CSS

```

var gulp = require('gulp');
var minifyHtml = require("gulp-minify-html");
var minifycss = require('gulp-clean-css');
//var minifycss = require("gulp-minify-css");
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
var path = require("path");
var glob = require("glob");
var watch = require('gulp-watch');

gulp.task('minify-html', function () {
    gulp.src('./*.html')
    .pipe(minifyHtml())
    .pipe(gulp.dest('build/'));
});

gulp.task('minify-css', function () {

	gulp.src('./css/*.css')
	.pipe(concat("finally.css"))
	.pipe(rename({ suffix: '.min' }))
	.pipe(minifycss())
	.pipe(gulp.dest('build/css'));

});

gulp.task('minify-js', function(){

	gulp.src(__dirname + "/js/*.js")
	.pipe(concat("finally.js"))
        .pipe(rename({ suffix: '.min' }))
	.pipe(uglify())
	.pipe(gulp.dest('build/js'))

});

gulp.task('default',function() {
    gulp.start('minify-css','minify-js');
});

gulp.task('watch', function() {
    watch(__dirname + "/css/**/*.css", function() {
        gulp.run('minify-css');
    });
    watch(__dirname + "/js/**/*.js", function() {
        gulp.run('minify-js');
    });
});

```

### 命令行传递参数

```

var gulp = require('gulp');
var argv = require('yargs').argv;
//var minifyHtml = require("gulp-minify-html");
var minifycss = require('gulp-clean-css');
//var minifycss = require("gulp-minify-css");
var spriter = require("gulp-spriter");
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');
var path = require("path");
var glob = require("glob");
var watch = require('gulp-watch');

//var src = __dirname + "/" + argv.stage + "/build/" + argv.src;
//var dest = __dirname + "/" + argv.stage + "/build/" + argv.src;
var src = __dirname + "/" + argv.src;
var dest = __dirname + "/dist/" + argv.src;
console.log(src);

gulp.task('minify-html', function () {
    gulp.src( src +'/*.html') // path to your files
    .pipe(minifyHtml())
    .pipe(gulp.dest( dest + '/'));
});

gulp.task('minify-css', function () {

	gulp.src(src + '/css/**/*.css')
	.pipe(concat("finally.css"))
	.pipe(rename({ suffix: '.min' }))
	.pipe(minifycss())
	.pipe(gulp.dest(dest + '/css'));

});

gulp.task('minify-js', function(){

	gulp.src(src + "/js/**/*.js")
	.pipe(concat("finally.js"))
        .pipe(rename({ suffix: '.min' }))
	.pipe(uglify())
	.pipe(gulp.dest( dest + '/js'))

});

gulp.task("spriter",["clean"],function(){
  return gulp.src( dest + "/css/finally.min.css")
         .pipe(spriter({
            sprite:"finally.png",
            slice: src + "/images",
            outpath: dest + "/images"
          }))
         .pipe(gulp.dest( dest + '/images'))
})

gulp.task('default',function() {
    gulp.start('minify-css','minify-js');
});

gulp.task('watch', function() {
    watch(src + "/css/**/*.css", function() {
        gulp.run('minify-css');
    });
    watch(src + "/js/**/*.js", function() {
        gulp.run('minify-js');
    });
});			

```

## 第 5 章 webpack

## 第 6 章 minifier

```

#!/bin/bash

cd /usr/local/src/
wget https://github.com/yui/yuicompressor/releases/download/v2.4.8/yuicompressor-2.4.8.jar
mv yuicompressor-2.4.8.jar /usr/local/libexec/

cat >> /usr/local/bin/yuicompressor <<'EOF'
java -jar /usr/local/libexec/yuicompressor-2.4.8.jar $@
EOF

chmod +x /usr/local/bin/yuicompressor

```

```

$ yuicompressor

YUICompressor Version: 2.4.8

Usage: java -jar yuicompressor-2.4.8.jar [options] [input file]

Global Options
  -V, --version             Print version information
  -h, --help                Displays this information
  --type <js|css>           Specifies the type of the input file
  --charset <charset>       Read the input file using <charset>
  --line-break <column>     Insert a line break after the specified column number
  -v, --verbose             Display informational messages and warnings
  -o <file>                 Place the output into <file>. Defaults to stdout.
                            Multiple files can be processed using the following syntax:
                            java -jar yuicompressor.jar -o '.css$:-min.css' *.css
                            java -jar yuicompressor.jar -o '.js$:-min.js' *.js

JavaScript Options
  --nomunge                 Minify only, do not obfuscate
  --preserve-semi           Preserve all semicolons
  --disable-optimizations   Disable all micro optimizations

If no input file is specified, it defaults to stdin. In this case, the 'type'
option is required. Otherwise, the 'type' option is required only if the input
file extension is neither 'js' nor 'css'.

```

## 第 7 章 CSS Frameworks

```

<style>
	html{filter:gray;}
	html{filter:progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);}
</style>

```

## 浏览器判断

```

<!--[if IE 8]>
  <link rel="stylesheet" type="text/css" href="ie8.css">
<![endif]-->
<!--[if IE 7]>
  <link rel="stylesheet" type="text/css" href="ie7.css">
<![endif]-->
<!--[if IE 6]>
  <link rel="stylesheet" type="text/css" href="ie6.css">
<![endif]-->

```

```

<!--[if lt IE 7 ]><html class="ie6" lang="zh-cn"><![endif]-->
<!--[if IE 7 ]><html class="ie7" lang="zh-cn"><![endif]-->
<!--[if IE 8 ]><html class="ie8" lang="zh-cn"><![endif]-->
<!--[if IE 9 ]><html class="ie9" lang="zh-cn"><![endif]-->

```

## 第 8 章 stylesheet

## Less

http://www.lesscss.net/

## 第 8 章 stylesheet

## css 冗余/废弃样式检查

https://code.google.com/archive/p/css-redundancy-checker/

```
wget https://storage.googleapis.com/google-code-archive-source/v2/code.google.com/css-redundancy-checker/source-archive.zip
unzip source-archive.zip
gem install hpricot 
ruby css-redundancy-checker.rb [cssfile] [directory of html files OR .txt file listing urls to use]

```

```
# vim url.txt
http://www.netkiller.cn/zh-cn/
http://www.netkiller.cn/index.html
http://www.netkiller.cn/zh-tw/index.html

ruby css-redundancy-checker.rb your.css url.txt

```

## 第 9 章 HTML

### *HTML XHTML HTML5*

## iPhone WebApp

### 移动设备开发

### 拨打电话

禁用电话号码识别

```

<meta name="format-detection" content="telephone=no"  />

```

声明电话链接

```

<a href="tel:13113668890">拨打电话</a>

```

### iphone 图标设置

```

<!DOCTYPE html>
<html manifest="iphone.manifest">
<head>
    <meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1.0, maximum-scale=1.0"/>
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="apple-mobile-web-app-status-bar-style" content="black" />
    <link rel="apple-touch-icon" href="apple-touch-icon.png"/>
    <link rel="apple-touch-startup-image" href="apple-touch-startup-image.png" />
    <link rel="stylesheet" href="iphone.css" type="text/css" media="screen, mobile" title="main" charset="utf-8">
    <title>offline neo</title>
</head>
<body>
    ...
    ...
    ...
  <script type="text/javascript" src="iphone.js"></script>
</body>
</html>

```

## frame

```

<!DOCTYPE html>
<html>
<head>
<title>HTML Frames</title>
</head>
<frameset rows="0,100%" frameborder="0">
   <frame name="top" src="" />
   <frame name="main" src="http://www.netkiller.cn/" />
   <noframes>
   <body>
      Your browser does not support frames.
   </body>
   </noframes>
</frameset>
</html>

```

## 第 10 章 HTML5

## header

```

<header class="post-header">
    <h1>title</h1>
    <p class="meta"></p>
</header>

```

## article

```

<article class="post-content">
	content
</article>

```

## 第 11 章 Javascript

## window

### window.location

href

```

var source=window.location.href;
if(source.indexOf('www.example.com')>0){
	... 
}		

```

hostname

```

if(window.location.hostname == "www.example.com"){
	...
}			

```

```

<span>网址： <script>document.write(window.location.hostname);</script></span>		

```

## navigator

### userAgent

```
document.write(navigator.userAgent);
document.write(navigator.userAgent.indexOf("MicroMessenger"));

```

```

<script>
var userAgent = window.navigator.userAgent.toLowerCase();
var tags = ["iphone", "android", "phone", "mobile", "wap", "netfront", "java", "opera mobi", "opera mini", "ucweb", "windows ce", "symbian", "series", "webos", "sony", "blackberry", "dopod", "nokia", "samsung", "palmsource", "xda", "pieplus", "meizu", "midp", "cldc", "motorola", "foma", "docomo", "up.browser", "up.link", "blazer", "helio", "hosin", "huawei", "novarra", "coolpad", "webos", "techfaith", "palmsource", "alcatel", "amoi", "ktouch", "nexian", "ericsson", "philips", "sagem", "wellcom", "bunjalloo", "maui", "smartphone", "iemobile", "spice", "bird", "zte-", "longcos", "pantech", "gionee", "portalmmm", "jig browser", "hiptop", "benq", "haier", "^lct", "320x320", "240x320", "176x220", "w3c ", "acs-", "alav", "alca", "amoi", "audi", "avan", "benq", "bird", "blac", "blaz", "brew", "cell", "cldc", "cmd-", "dang", "doco", "eric", "hipt", "inno", "ipaq", "java", "jigs", "kddi", "keji", "leno", "lg-c", "lg-d", "lg-g", "lge-", "maui", "maxo", "midp", "mits", "mmef", "mobi", "mot-",
                                "moto", "mwbp", "nec-", "newt", "noki", "oper", "palm", "pana", "pant", "phil", "play", "port", "prox", "qwap", "sage", "sams", "sany", "sch-", "sec-", "send", "seri", "sgh-", "shar", "sie-", "siem", "smal", "smar", "sony", "sph-", "symb", "t-mo", "teli", "tim-", "tsm-", "upg1", "upsi", "vk-v", "voda", "wap-", "wapa", "wapi", "wapp", "wapr", "webc", "winw", "winw", "xda", "xda-", "Googlebot-Mobile"];

console.log(userAgent);

for (var i = 0; i < tags.length; i++) {
        var tag = tags[i];
        //document.write(tag);
        if(userAgent.indexOf(tag) !== -1) {
                //console.log(tag);
                var hostname = document.location.hostname;
                var domain = hostname.substring(hostname.lastIndexOf(".", hostname.lastIndexOf(".") - 1) + 1);
                //document.write(domain);
                document.location = "//m."+domain;      
        }
}

</script>			

```

## document

### referrer

referrer

```
javascript:alert(document.referrer);

```

### domain

document.domain;

去掉主机，例如 www

```

document.domain.split(".").slice(-2).join(".");			

```

## String 字符串处理

### JSON.parse

```

var json = '{"result":true,"count":1}',
    obj = JSON.parse(json);

alert(obj.count);

```

### replace 替换

正则替换手机号码

```

    var str = "13113668890"; 
    var res = str.replace(/([0-9]{1,3})([0-9]{1,4})([0-9]{1,4})/, "$1****$3");

```

## Date and Time

```
var dateObject=new Date();
document.writeln(dateObject.toDateString());
Mon Mar 28 2016

document.writeln(dateObject.toLocaleDateString());
‎2016‎年‎3‎月‎28‎日

document.writeln(dateObject.toISOString())
2016-03-28T08:57:30.244Z

document.writeln(dateObject.toISOString().slice(0,10));
2016-03-28

document.writeln(dateObject.toISOString().slice(11,19));
09:11:12

document.writeln(dateObject.toTimeString().slice(0,9));
17:16:11

document.writeln(new Date("2016-3-30").getTime());
1459267200000

```

```
var today = new Date();
var h = today.getHours();
var m = today.getMinutes();
var s = today.getSeconds();

```

例 11.1. 倒数计时例子

```

function checkTime(i)    
{    
   if (i < 10) {    
       i = "0" + i;    
    }    
   return i;    
}

var ts = (new Date("2016-3-30")) - (new Date());//计算剩余的毫秒数  
var dd = parseInt(ts / 1000 / 60 / 60 / 24, 10);//计算剩余的天数  
var hh = parseInt(ts / 1000 / 60 / 60 % 24, 10);//计算剩余的小时数  
var mm = parseInt(ts / 1000 / 60 % 60, 10);//计算剩余的分钟数  
var ss = parseInt(ts / 1000 % 60, 10);//计算剩余的秒数  
day = checkTime(dd);  
hour = checkTime(hh);  
minute = checkTime(mm);  
second = checkTime(ss);  

document.writeln(day + "天" + hour + "时" + minute + "分" + second + "秒");  			

```

## from 表单相关事件

### onblur

转换为小写字母

```

<input type="text" id="email" name="form.email" value="${form.email}" onblur="this.value = this.value.toLowerCase();" maxlength="30" class="" tabindex="94"/>	

```

## 禁止复制与鼠标右键

```

<script language="JavaScript" type="text/javascript">
document.oncontextmenu=new Function("event.returnValue=false;");
document.onselectstart=new Function("event.returnValue=false;");
</script>

```

## DOMDocument

### createTextNode

<SCRIPT>
function fnChangeNode(){
   var oTextNode = document.createTextNode("文本节点已创建");
   var oReplaceNode = oSpan.childNodes(0);
   oReplaceNode.replaceNode(oTextNode);
}
</SCRIPT>

<span ID="oSpan" onclick="fnChangeNode()">
点击此处
</span>

## Microsoft.XMLHTTP

### Get

```

<script type="text/javascript" language="javascript">
    var http_request = false;
    function makeRequest(url) {

        http_request = false;

        if (window.XMLHttpRequest) { // Mozilla, Safari,...
            http_request = new XMLHttpRequest();
            if (http_request.overrideMimeType) {
                http_request.overrideMimeType('text/xml');
            }
        } else if (window.ActiveXObject) { // IE
            try {
                http_request = new ActiveXObject("Msxml2.XMLHTTP");
            } catch (e) {
                try {
                    http_request = new ActiveXObject("Microsoft.XMLHTTP");
                } catch (e) {}
            }
        }

        if (!http_request) {
            alert('Giving up :( Cannot create an XMLHTTP instance');
            return false;
        }
        http_request.onreadystatechange = alertContents;
        http_request.open('GET', url, true);
        http_request.send(null);
    }

    function alertContents() {

        if (http_request.readyState == 4) {
            if (http_request.status == 200) {
                alert(http_request.responseText);
            } else {
                alert('There was a problem with the request.');
            }
        }

    }
</script>
<span
    style="cursor: pointer; text-decoration: underline"
    onclick="makeRequest('http://127.0.0.1/tmp/xml/test.php')">
        Make a request
</span>

```

### POST

```

<script type="text/javascript" language="javascript">
    var http_request = false;
    function makeRequest(url) {

        http_request = false;

        if (window.XMLHttpRequest) { // Mozilla, Safari,...
            http_request = new XMLHttpRequest();
            if (http_request.overrideMimeType) {
                http_request.overrideMimeType('text/xml');
            }
        } else if (window.ActiveXObject) { // IE
            try {
                http_request = new ActiveXObject("Msxml2.XMLHTTP");
            } catch (e) {
                try {
                    http_request = new ActiveXObject("Microsoft.XMLHTTP");
                } catch (e) {}
            }
        }

        if (!http_request) {
            alert('Giving up :( Cannot create an XMLHTTP instance');
            return false;
        }
        http_request.onreadystatechange = alertContents;

        attr = 'name=neo&nickname=netkiller';
        http_request.open('POST', url, true);
        http_request.setRequestHeader ("Content-Length",attr.length);
		http_request.setRequestHeader ("CONTENT-TYPE","application/x-www-form-urlencoded");
        http_request.send(attr);

    }

    function alertContents() {

        if (http_request.readyState == 4) {
            if (http_request.status == 200) {
                alert(http_request.responseText);
            } else {
                alert('There was a problem with the request.');
            }
        }

    }
</script>
<input type="text" name="textbox">
<br>
<span
    style="cursor: pointer; text-decoration: underline"
    onclick="makeRequest('http://127.0.0.1/tmp/xml/test.php')">
        Make a request
</span>

```

## jQuery

过程 11.1. 

### Selectors(选择器)

```

    if(window.location.hostname.indexOf("example.com") !== -1 ){
    	$("#nav3").hide();
    	$("#platform-nav li:nth-child(3)").hide();
    	$("#platform-nav li:nth-child(4)").hide();
    	$(".footer .fcon .coll").hide();
    	$(".footer .fcon .col3").hide();
    	$(".footer .fcon .col4").hide();
    }

```

### jQuery 属性操作

#### is

```

<a id="startMenu" href="#" class="more">

<section id="menu" class="right_menu disnone">
  <dl>
    <dt><i></i></dt>
    <Dd>您好！创富金融欢迎您</Dd>
  </dl>
  <ul>
    <li><a href="#">首页</a></li>
    <li><a href="#">简介</a></li>
    <li><a href="#">...</a></li>
    <div style="clear:both;"></div>
  </ul>
</section>

<script>
	$(document).ready(function() {

		$("#startMenu").click(function() {
			if($("#menu").is(":visible")){
				$("#menu").hide();	
			}else{
				$("#menu").show();
			}

		});
	});
</script>

```

#### css

```

$("button").click(function(){
  $("p:first").addClass("intro");
});

$( "p" ).removeClass( "myClass yourClass" )
$( "p" ).removeClass( "myClass noClass" ).addClass( "yourClass" );

<p>hello</p>

<p id="hello">hello</p>
<script type="text/javascript">

$("p").addClass("Helloworld");
$("#hello").addClass("Helloworld");

</script> 

```

### 时间触发

#### setTimeout 定时执行一次

```
$(document).ready(function(){
	setTimeout(function(){
		$("#error").hide();
	},3000);
});

```

#### setInterval 间隔执行

```
$(document).ready(function(){
	setInterval(function(){
		alert("test");
	},3000);
});

```

### text

```

<p>hello</p>

<p id="hello">hello</p>
<script type="text/javascript">

$("p").text("Helloworld");
$("#hello").text("Helloworld");

</script> 

```

### inArray

返回值是数组的 key

```
	var host = window.location.hostname;
	var domains = ["netkiller.github.io","www.netkiller.cn"];

	if(jQuery.inArray( host, domains ) != -1) {
	...
	...		
	}

```

### Ajax

#### Load

#### GET

```

jQuery.ajax({
	type:"GET",
	url: "/path/to/url",
	data: "code="+code,
	success:function(data){
		if(data.status){
			alert(data.text)
		}
	},
	error: function(){
	}
}); 			

```

#### Post

#### jsonp

```

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.3.2/jquery.min.js"></script>
<script>
$.getJSON('http://www.foobar.com/json.php?callback=?', function(data){
alert(data.foo);
});
</script>

<?php
echo $_GET['callback'], '(', json_encode(array('foo' => 'bar')), ')';
?>

```

```

// Using YQL and JSONP
$.ajax({
    url: "http://query.yahooapis.com/v1/public/yql",

    // The name of the callback parameter, as specified by the YQL service
    jsonp: "callback",

    // Tell jQuery we're expecting JSONP
    dataType: "jsonp",

    // Tell YQL what we want and that we want JSON
    data: {
        q: "select title,abstract,url from search.news where query=\"cat\"",
        format: "json"
    },

    // Work with the response
    success: function( response ) {
        console.log( response ); // server response
    }
});

```

#### No 'Access-Control-Allow-Origin' header is present on the requested resource.

原因是 ajax 跨域请求造成

将 dataType: 'JSON' 替换为 dataType: 'JSONP'

#### 同步 AJAX

Jquery ajax 请求默认是异步方式，通过 async: false, 参数修改为同步模式。

```

	function exchange(money){
			var amount = 0;
			jQuery.ajax({
				type: "GET",
				url: "ajax.php?money=" + money,
				dataType: "json",
				async: false,
				data: "",
				success: function (data) {
					if (data.amount) {
						amount = data.amount
					}
				},
				error: function () {
				}
			});
		return amount;
		}		

```

### Form 表单处理

#### select

```

	var select = $('#bank');
	//$('option', select).remove();
	$.each(banklist, function(key,code) {
		var option = new Option(key, code)
	    select.append( option );
	});			

	$('#bank').append($("<option></option>").attr("value","CMB").text("招商银行"));

```

#### input

设置 value

```
$("#amount").val("100");

```

### Jquery 事件

#### click 事件

```

$(document).ready(function() {

	$("#Button1").click(function() {
		$("#Tab1").hide();
		$("#Tab2").show();
	});
	$("#Button2").click(function() {
		$("#Tab1").show();
		$("#Tab2").hide();
	});	
});			

```

解除事件绑定

```

$( "#Button1").unbind( "click" );
$( "#Button2").unbind( "click" );		

```

事件中绑定事件

```

$("#Button").click(function() {	
	$( "#Button1").unbind( "click" );
	$( "#Button2").unbind( "click" );

	$("#Button1").click(function() {
		$("#Tab1").hide();
		$("#Tab2").show();
	});
	$("#Button2").click(function() {
		$("#Tab1").show();
		$("#Tab2").hide();
	});	
});	

```

### Garlic.js - 表单数据持久化

http://garlicjs.org/

Garlic.js 可以让你自动的持久化表单中的数据到本地，直到表单被提交。这样用户就不用担心因为误操作导致表单输入的数据丢失。

使用方法很简单：

```

<script type="text/javascript">
  $( 'form' ).garlic();
</script>

```

## Bootstrap

http://twbs.github.io/bootstrap/

## ActiveWidgets - WebUI

## Highslide

[`highslide.com/`](http://highslide.com/)

Highslide JS is an image, media and gallery viewer written in JavaScript.

## JavaScript 代码混淆

### JavaScript Packer

[`joliclic.free.fr/php/javascript-packer/index.php`](http://joliclic.free.fr/php/javascript-packer/index.php)

## phantomjs - headless WebKit with JavaScript API

## Javascript MVC Frameworks

http://codebrief.com/2012/01/the-top-10-javascript-mvc-frameworks-reviewed/

### Backbone

http://backbonejs.org/

### example

http://todomvc.com/

## 第 12 章 SSI

## SSI 环境变量

显示所有环境变量

```

<!--#printenv -->

```

```
HTTP_USER_AGENT=curl/7.29.0
HTTP_ACCEPT=*/*
LAST_MODIFIED=Tuesday, 03-Nov-2015 09:57:28 HKT
DOCUMENT_URI=/
REMOTE_PORT=37482
SERVER_NAME=224.25.22.70
SERVER_SOFTWARE=Apache Tomcat/7.0.65 OpenJDK 64-Bit Server VM/20.0-b12 Linux
SCRIPT_FILENAME=/srv/apache-tomcat/webapps/ROOT/index.html
DATE_LOCAL=Tuesday, 03-Nov-2015 09:57:31 HKT
SERVER_ADDR=224.25.22.70
SERVER_PROTOCOL=HTTP/1.1
REQUEST_METHOD=GET
DOCUMENT_NAME=
SERVER_PORT=8080
SCRIPT_NAME=/index.html
REMOTE_ADDR=202.130.11.34
DATE_GMT=Tuesday, 03-Nov-2015 01:57:31 GMT
REMOTE_HOST=202.130.101.34
HTTP_HOST=224.25.22.70:8080
QUERY_STRING=
GATEWAY_INTERFACE=CGI/1.1
org.apache.catalina.ssi.SSIServlet=true
REQUEST_URI=/

```

### QUERY_STRING GET 参数传递

例如我们需要实现一个功能，test.html?后面的参数需要传递到页面中。

```

http://www.netkiller.cn/lp/test.html?utm_source=ss&utm_medium=baidusem&utm_campaign=lpgrant			

```

```

<a href="<!--#echo var="WWW_URL"-->/customer/CreateAccount.do?<!--#echo var="QUERY_STRING"-->">新建用户</a>

```

### SERVER_NAME 与 HTTP_HOST

```
server {
    listen      80;
    listen 		443 ssl http2;
    server_name api.netkiller.com api.neo.com api.chen.com;
}

```

SERVER_NAME 如果一个主机配置多个域名，那么 SERVER_NAME 是域名列表中的第一个域名 api.netkiller.com

HTTP_HOST 是当前进入网站的域名

## set

设置环境变量

```

<!--#set var="foo" value="Bar" -->
<!--#echo var="foo"-->

```

环境变量

```

<!--#set var="WWW_URL"      value="//${SERVER_NAME}"-->
<!--#set var="IMG_URL"      value="//${SERVER_NAME}"-->

```

## echo

显示环境变量

```

<!--#echo var="SERVER_NAME"--> <br />
<!--#echo var="DOCUMENT_URI"--> <br />
<!--#echo var="HTTP_HOST"--> <br />
<!--#echo var="SERVER_PORT"--> <br />

```

默认值

```

<!--# echo var="name" default="neo" -->		

```

禁止编码数据，例如下面 LIVE800_URL 显示出来后&会被编码.

```

<!--#set var="LIVE800_URL" value="//${SERVER_NAME}/index.jsp?id=111&pid=122&cid=222"-->
<!--#echo var='LIVE800_URL' default='' encoding='none'-->		

```

## 包含网页

```

<!--#include virtual="file-name" -->		

```

包含一个配置文件

```

<!--# if expr="${SERVER_NAME}=/^(www|images|info|myid|ad).example.com.*/" -->
	<!--#include file="/include/cn/config.html"-->
<!--# else -->
	<!--#include virtual="/include/cn/config.html"-->
<!--# endif -->		

```

## if 条件判断

```

<!--# if expr="$name" -->
	<!--# echo var="name" -->
<!--# else -->
	netkiller
<!--# endif -->

```

```

<!--#config timefmt="%A" -->
<!--#if expr="$DATE_LOCAL = /Monday/" -->
<p>Meeting at 10:00 on Mondays</p>
<!--#elif expr="$DATE_LOCAL = /Friday/" -->
<p>Turn in your time card</p>
<!--#else -->
<p>Yoga class at noon.</p>
<!--#endif -->

```

```

<!--#if expr="${SERVER_NAME}=/^(www|images|info|myid|ad).mydomain.com.*/" -->

    <!--#set var="WWW_URL" 	value="http://www.mydomain.com"-->
    <!--#set var="NEWS_URL" value="http://news.mydomain.com"-->
    <!--#set var="IMG_URL" 	value="http://img.mydomain.com"-->
    <!--#set var="JS_URL" 	value="http://img.mydomain.com/js"-->
    <!--#set var="CSS_URL" 	value="http://img.mydomain.com/css"-->

<!--#else -->

...
...

<!--#endif -->

<!--#if expr="${DOCUMENT_URI}=/\/cn\/.*/"-->
        <!--#set var="LANG" value="cn"-->
<!--#elif expr="${DOCUMENT_URI}=/\/tw\/.*/"-->
        <!--#set var="LANG" value="tw"-->
<!--#elif expr="${DOCUMENT_URI}=/\/en\/.*/"-->
        <!--#set var="LANG" value="en"-->
<!--#endif-->

<!--# if expr="${SERVER_NAME}=/.*.example.com/" -->
    <!--#set var="WWW_URL"      value="//www.example1.com"-->
    <!--#set var="CSS_URL"      value="//css.example1.com"-->
    <!--#set var="IMG_URL"      value="//img.example1.com"-->
<!--# else -->
    <!--#set var="WWW_URL"      value="//www.example.com"-->
    <!--#set var="IMG_URL"      value="//img.example.com"-->
<!--# endif --> 

```

判断 HTTP 与 HTTPS

```

<!--#set var="HTML_HOST" value="http://www.example.com"-->
<!--#set var="INFO_HOST" value="http://info.example.com"-->
<!--#set var="NEWS_HOST" value="http://news.example.com"-->

<!--#if expr="${SERVER_PORT}=/443/"-->

<!--#set var="MYID_HOST" value="https://myid.example.com"-->
<!--#set var="IMG_HOST" value="https://myid.example.com/images"-->
<!--#set var="JS_HOST" value="https://myid.example.com/images"-->
<!--#set var="CSS_HOST" value="https://myid.example.com/images"-->

<!--#else -->

<!--#set var="MYID_HOST" value="http://myid.example.com"-->
<!--#set var="IMG_HOST" value="http://images.example.com"-->
<!--#set var="JS_HOST" value="http://images.example.com"-->
<!--#set var="CSS_HOST" value="http://images.example.com"-->

<!--#endif -->

<!--#set var="IMAGE_POST_HOST" value="http://card-up.example.com:4141"-->
<!--#set var="IMAGE_UPLOAD_HOST" value="http://card-look.example.com:4242"-->

<!--#if expr="${DOCUMENT_URI}=/\/cn\/.*/"-->
        <!--#set var="LANG" value="cn"-->
<!--#elif expr="${DOCUMENT_URI}=/\/tw\/.*/"-->
        <!--#set var="LANG" value="tw"-->
<!--#elif expr="${DOCUMENT_URI}=/\/en\/.*/"-->
        <!--#set var="LANG" value="en"-->
<!--#endif-->

```

判断是否经过反向代理

```

<!--#if expr="${X_FORWARDED_FOR}"-->

<!--#set var="IMG_HOST" value="/images"-->
<!--#set var="JS_HOST" value="/images"-->
<!--#set var="CSS_HOST" value="/images"-->

<!--#else -->

<!--#set var="IMG_HOST" value="http://images.example.com"-->
<!--#set var="JS_HOST" value="http://images.example.com"-->
<!--#set var="CSS_HOST" value="http://images.example.com"-->

<!--#endif -->		

```

&& 操作

```

<!--#if expr="(${HTTP_USER_AGENT} = /Mozilla\/4/) && (${HTTP_USER_AGENT} != /MSIE/)" -->
 Netscape styles
<!--#elif expr="(${HTTP_USER_AGENT} = /Mozilla\/4/) && (${HTTP_USER_AGENT} = /MSIE/)" -->
MSIE styles
<!--#else -->
You must be using Opera or other?
<!--#endif -->		

```

## FAQ 常见问题

### SERVER_NAME 与 HTTP_HOST 有什么不同？

SERVER_NAME 与 HTTP_HOST 有什么不同，下面是 nginx 配置:

```
server {
    listen       80 ;
    server_name www.example.com example.com www.netkiller.cn;

    charset utf-8;
    access_log  /var/log/nginx/www.example.com.access.log;
    error_log  /var/log/nginx/www.example.com.error.log;

    if ($query_string = "") {
       set $args "";
    }

    location / {
        root /www/example.com/www.example.com;
        index index.html;
	}
}

```

当你使用上面的域名访问服务器时 SERVER_NAME 取到的永远是 server_name 配置的第一个域名，即：www.example.com

而 HTTP_HOST 是你浏览器 URL 上面的域名

## 第 13 章 Theme & UI

## bootstrap

http://twitter.github.com/bootstrap/index.html

## 第 14 章 3rd party

## Share Buttons

### Share Buttons, Share Plugin, Share Analytics, Media Solutions

http://sharethis.com

## discussions

http://disqus.com/

## Highlight

### SyntaxHighlighter

http://alexgorbatchev.com/SyntaxHighlighter/

### highlight.js

https://highlightjs.org

## 所见即所得现在编辑工具

### FCKeditor

### NicEdit

### TinyMCE

### WYSIWYG

### Quill

http://quilljs.com/

## 第 15 章 Div+CSS 页面设计

最近几年，随着业界越来越关注 XHTML+CSS 的标准化设计，一个新兴职业已经诞生，这就是“网站重构师”，这个新兴职业人才紧缺，他们主要的职责是将 HTML+Table+Javascript 的架构向 XHTML+CSS+Ajax 迁移。

## 页面元素命名

```

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>恒信贵金属报表系统</title>
    </head>
    <body>
    	<div id="header">
    		<div id="logo"></div>
    		<div id="banner"></div>
    		<div id="navigation"></div>
    	</div>
    	<div id="wrapper">
    		<div id="sidebar"></div>
    		<div id="content"></div>
    	</div>
    	<div id="footer">
    		<div id="footer-nav"></div>
    		<div id="copyright"></div>
    		<div id="legal"></div>
    	</div>
    </body>
<html>

```

## XHTML+DIV+CSS

为何使用表格排版是不明智的选择？为什么要选择 DIV+CSS？

首选我来说说表格排版，表格排版也是有好处的，一是排版速度快，二是兼容性比 CSS 好。做为一般的小网站还是比较适合的，如果在大型网站使用表格就不太合适。 表格必须定义很多属性如 width="100%" border="0" cellpadding="0" cellspacing="0"，并且有时候 tr 标签显得多余。

例 15.1. 表格排版范例

```

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html >
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

<title>Table Example</title>
</head>

<body>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
  <tr>
    <td>Logo</td>
    <td>Banner</td>
  </tr>
</table>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
  <tr>
    <td>Home</td>
    <td>News</td>
    <td>Contact</td>
    <td>About</td>
  </tr>
</table>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
  <tr>
    <td><table width="100%" border="0" cellspacing="0" cellpadding="0">
      <tr>
        <td>Top 10 </td>
      </tr>
      <tr>
        <td>xxxxxxxx</td>
      </tr>
      <tr>
        <td>xxxxxxxx</td>
      </tr>
      <tr>
        <td>xxxxxxxx</td>
      </tr>
      <tr>
        <td>xxxxxxxx</td>
      </tr>
    </table>
      <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
          <td>Link </td>
        </tr>
        <tr>
          <td>xxxxxxxx</td>
        </tr>
        <tr>
          <td>xxxxxxxx</td>
        </tr>
        <tr>
          <td>xxxxxxxx</td>
        </tr>
        <tr>
          <td>xxxxxxxx</td>
        </tr>
      </table></td>
    <td><table width="100%" border="0" cellspacing="0" cellpadding="0">
      <tr>
        <td align="center">Article Title </td>
      </tr>
      <tr>
        <td><p>Contect</p>
          <p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
          <p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
          <p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
          <p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p></td>
      </tr>
      <tr>
        <td>Feedback</td>
      </tr>
    </table></td>
  </tr>
</table>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
  <tr>
    <td align="center">Copyright XXXX </td>
  </tr>
</table>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
</body>
</html>

```

你可以对比上面看看 div+css 是如何规划版面，并且 css 很多定义是可以重用的。

例 15.2. XHTML+DIV+CSS 排版范例

```

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html >
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>hello world</title>
<style type="text/css">
<!--
body{
	width: 795px;
}
div1{
	border-color: #119EBA;
	border-width: 1px;
	border-style: solid;
	margin: 5px;
}
#header{
}
#logo, #banner{
	float:left;
	height: 75px;
}
#nav{
	clear:both;
}
#nav ul {
	list-style-type:none;
	margin: 0px;
	padding:0px;
}
#nav ul li{
	float:left;
	width: 100px;
}
#main{ clear:both;}
#main #left {
	float:left;
	width: 30%;
}
#main #right {
	float:right;
	width: 70%;
}
.box{}
.box h2 {
	margin: 0;
	padding: 0px;
}
.box a { display:block;}
#footer{ clear:both}
#footer #copyright {
	text-align:center;
}

.article {
	border-color: black;
	border-width: 1px;
	border-style: solid;
	margin: 5px;
	padding: 10px;
}
.article .article_title{
	font-size: 24px;
	font-weight:bold;
	text-align:center;
}
.article .article_content{ font-size:10px;}
-->
</style>
</head>

<body>

<div id="header">
	<div id="logo"> Logo </div>
	<div id="banner"> Banner </div>
</div>

<div id="nav">
	<ul>
		<li><a href="#"> Home </a></li>
		<li><a href="#"> News </a></li>
		<li><a href="#"> Person </a></li>
		<li><a href="#"> Group </a></li>
		<li><a href="#"> Network </a></li>
	</ul>
</div>

<div id="main">
	<div id="left">
		<div class="box">
			<h2>title</h2>
			<a href="#"> link </a>
			<a href="#"> link </a>
			<a href="#"> link </a>
		</div>

		<div class="box">
			<h2>title</h2>
			<a href="#"> link </a>
			<a href="#"> link </a>
			<a href="#"> link </a>
		</div>
	</div>
	<div id="right">
		<div class="article">
			<div class="article_title">
				Article Title
			</div>
			<div class="article_content">
				<p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
				<p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
				<p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
				<p>XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</p>
			</div>
		</div>
	</div>
</div>

<div id="footer">
	<div id="copyright"> Copyright Neo Chan</div>
</div>

</body>
</html>

```

上面例子我们可以看到 div 与 table 相比所使用的标签更少，无形中给网站减了肥。

CSS 的 class,id 名称定义规范：

1.  一定要简单，可读例如 header，footer

2.  对于在页面中不重复，自始至终只出现一次可定义为 id，例如 id="header"，id="footer"

3.  对于在页面中经常重复出现的，可定义为 class，例如 id="article_block"，id="news_block"

### 注意

不要使用 HTML 属性，尽量使用 css。 herf,src,class,id 等属性除外。

下面是一个例子

```

<font color="red" size="12" face="Arial, Helvetica, sans-serif" >Hello workd</font>

```

你应该使用 CSS 实现,如果能使用 CSS 实现尽量不要多用一条 HTML 和属性。

```

<style type="text/css">
.hello{
	color:red;
	font-size:12px;
	font-family:Arial, Helvetica, sans-serif;
}
</style>
<div id="hello">Hello workd</font>

```

## 页面结构设计

页面结构从上到下依次是

*   header 主要包括导航，登录，Logo, Banner

*   body 网站主要内容，并且还可以分为左右两栏，左中右三栏。

*   footer 导航，版权

header,footer 将显示在所有页面，一般很少改动。

### Home page (首页)

```

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html >
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Title</title>
</head>

<body>

<div id="header">
	<div id="logo"></div>
    <div id="banner"></div>
    <div id="nav"></div>
</div>

<div id="page1">
 	<div class="left_nav"></div>
    <div class="right"></div>
</div>

<div id="page2">

</div>

<div id="page3">

</div>

<div id="footer">
  <div id="footer_nav"></div>
  <div id="copyright"></div>
</div>

</body>
</html>

```

page1 打开首页看到的第一屏页面，page2，page3 需要按翻页键

不要将 page1，page2，page3 放到一个 DIV 中

### 导航烂

```

<table>
	<tr>
		<td>Home</td>
		<td>News</td>
		<td>About</td>
		<td>Contact</td>
	</tr>
</table>

```

```

<div id="nav">
	<ul>
		<li><a href="#"> Home </a></li>
		<li><a href="#"> News </a></li>
		<li><a href="#"> Person </a></li>
		<li><a href="#"> Group </a></li>
		<li><a href="#"> Network </a></li>
	</ul>
</div>

```

### Left Bar

```

	<div id="left">
		<div class="box">
			<h2>title</h2>
			<a href="#"> link </a>
			<a href="#"> link </a>
			<a href="#"> link </a>
		</div>

		<div class="box">
			<h2>title</h2>
			<a href="#"> link </a>
			<a href="#"> link </a>
			<a href="#"> link </a>
		</div>
	</div>

```

### 区块设计 Block

网站经常用一些方块规划版面。

*   一种是矩形方框

*   另一种是有标题，标题下方是矩形方框

*   现在流行的是标题栏有多个选项卡，标题下方是矩形方框，当选择不同标题时，矩形方框中的内容随之改变。

传统方法如下：

例 15.3.  例子

table block example

```

<table>
	<tr>
		<td>
			内容
		</td>
	</tr>
</table>

```

div+css block example

```

<div class="simple_box">
			内容
</table>

```

例 15.4.  例子

table title block example

```

<table>
	<tr>
		<td>Top 10</td>
	</tr>
	<tr>
		<td>
			<table>
				<tr>
					<td>No.1</td>
				</tr>
				<tr>
					<td>No.2</td>
				</tr>
				<tr>
					<td>No.3</td>
				</tr>
				<tr>
					<td>No.n</td>
				</tr>
			</table>
		</td>
	</tr>
</table>

```

div+css title block example

```

<div class="title_block">
	<h2>
		Title
	<h2>
	<div>
		Content
	</div>
</div>

```

使用 dl 标记实现

```

<dl class="title_block">
	<dt>Title<dt>
	<dd>
		Content
	</dd>
</dl>

```

## 表格

这里的表格不是指用于排版，而是表格数据。

```

<style type="text/css">
.hello{
	width: 100%;
}
.hello tr{

}
.hello td{

}
</style>
<table class="mytable">
	<tr>
		<td></td>
	</tr>
</table>

```

## 图片优化

### onMouseOver/onMouseOut

我们在网站冲浪常常看会看到很多图片按钮，当鼠标入上去或鼠标移开图片会随之改变，这个的按钮至少需要三张小图片才能实现这样的功能。

我先说说这样做的缺点

*   三张图片，你的浏览器会启动三个线程链接你的图片服务器，不划算。

*   一旦其中一幅图片下载过程中中断，用户当把鼠标放到按钮上时，可能会出现一个红叉叉。

*   图片太多不好维护，易产生垃圾，占用磁盘空间，linux ext3 一个空文件占用 2048

最优方法是使用一张图片，将三幅图片平行或垂直排开，放到一幅图片中，然后使用 CSS 控制显示你需要的部分。

### 使用一幅图片处理 BLOCK 四角

corner.gif

![](img/corner.gif)

stylesheet

```

<style type="text/css">
<!--

.clear { clear: both; height: 0; font-size: 0; line-height: 0; zoom: 1 }

.containerPlain {
	background-color: #fff;
	border-right: 1px solid #cacaca;
	border-left: 1px solid #cacaca;
	padding: 0 3px;
}

.left_top_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: top left;
	float: left;
	font-size: 0;
}

.right_top_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: top right;
	float: right;
	font-size: 0;
}

.left_bottom_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: bottom left;
	float: left;
	font-size: 0;
}

.right_bottom_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: bottom right;
	float: right;
	font-size: 0;
}
.left_bottom_corner, .right_bottom_corner ,
.left_top_corner, .right_top_corner{
	background-image: url(corners/corner.gif);
}

.middle_top_line {
	display: block;
	float: left;
	height: 3px;
	line-height: 0;
	border-top: 1px solid #cacaca;
}

.middle_bottom_line {
	display: block;
	float: left;
	height: 3px;
	border-bottom: 1px solid #cacaca;
	font-size: 0;
}

.middle_top_line, .middle_bottom_line {
		width: 167px;
}

-->
</style>

```

HTML

```

<div style="width:175px;">
	<span class="left_top_corner"></span> <span class="middle_top_line"></span> <span class="right_top_corner"></span>
	<div class="containerPlain">
    	You Content
	</div>
	<span class="left_bottom_corner"></span> <span class="middle_bottom_line"></span> <span class="right_bottom_corner"></span>
</div>

```

下面是一个更复杂的例子

*   corner.gif
*   block_title_left.gif
*   block_title_right.gif

![](img/corner.gif)![](img/block_title_left.gif)![](img/block_title_right.gif)

stylesheet

```

<style type="text/css">
<!--

.clear { clear: both; height: 0; font-size: 0; line-height: 0; zoom: 1 }

.containerPlain {
	background-color: #fff;
	border-right: 1px solid #cacaca;
	border-left: 1px solid #cacaca;
	padding: 0 3px;
	clear: both;
}

.left_top_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: top left;
	float: left;
	font-size: 0;
}

.right_top_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: top right;
	float: right;
	font-size: 0;
}

.left_bottom_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: bottom left;
	float: left;
	font-size: 0;
}

.right_bottom_corner {
	display: block;
	width: 4px;
	height: 4px;
	background-position: bottom right;
	float: right;
	font-size: 0;
}
.left_bottom_corner, .right_bottom_corner ,
.left_top_corner, .right_top_corner{
	background-image: url(corners/corner.gif);
}

.middle_top_line {
	display: block;
	float: left;
	height: 3px;
	line-height: 0;
	border-top: 1px solid #cacaca;
}

.middle_bottom_line {
	display: block;
	float: left;
	height: 3px;
	border-bottom: 1px solid #cacaca;
	font-size: 0;
}

.middle_top_line, .middle_bottom_line {
		width: 167px;
}

.block_title {
	line-height: 26px;
	height: 26px;
	background-image: url(corners/block_title_left.png);
	background-repeat: no-repeat;
	padding-left: 10px;
	font-size: 13px;
	font-weight: bold;
	background-color: #dddbdc;
}

.block_title_right {
	display: block;
	background-image: url(corners/block_title_right.png);
	background-repeat: no-repeat;
	background-postition: right;
	float: right;
	width: 4px;
	height: 26px;
}
-->
</style>

```

HTML

```

<div style="width:175px;">
  <span class="left_top_corner"></span> <span class="middle_top_line"></span> <span class="right_top_corner"></span>
  <div class="containerPlain">
    <div class="block_title">
		<span class="block_title_right"></span> Title
    </div>
    <div style="padding: 10px 7px 7px 7px">
		Content
	</div>
  </div>
  <span class="left_bottom_corner"></span> <span class="middle_bottom_line"></span> <span class="right_bottom_corner"></span>
</div>

```

### 图片用背景图代替 img 标记

```

图片用背景图代替<img src="">

```

### 合并图片

下面是摘取 LinkedIn 网页,作为例子.

合并多张小图片为一张图片,然后通过偏移量取其中一部分显示,这样做的目的是,加快浏览器下载速度,降低与服务器建立连接的开销.

![](img/sprite3.png)

```

<!DOCTYPE html>
<html  dir="ltr" lang="en-US">
<head profile="http://gmpg.org/xfn/11">
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>LinkedIn Blog</title>
	<style type="text/css">
/*
Theme Name:  LinkedIn Blog
Theme URI:   http://blog.linkedin.com/
Description: LinkedIn's main blog theme
Author:      Prajakta Godbole
Author URI:  http://linkedin.com/
Version:     2.0
*/

/*
Reset styles
Copyright (c) 2011, Yahoo! Inc. All rights reserved.
Code licensed under the BSD License:
http://developer.yahoo.com/yui/license.html
version: 2.9.0
*/
html{color:#000;background:#FFF}body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,button,textarea,select,p,blockquote,th,td{margin:0;padding:0}table{border-collapse:collapse;border-spacing:0}fieldset,img{border:0}address,button,caption,cite,code,dfn,em,input,optgroup,option,select,strong,textarea,th,var{font:inherit}del,ins{text-decoration:none}li{list-style:none}caption,th{text-align:left}h1,h2,h3,h4,h5,h6{font-size:100%;font-weight:normal}q:before,q:after{content:''}abbr,acronym{border:0;font-variant:normal}sup{vertical-align:baseline}sub{vertical-align:baseline}legend{color:#000}

/* Colors and fonts */
html { background-color: #F5F5F5; }
body { font-family: Arial, Helvetica, "Nimbus Sans L", sans-serif; padding-top: 20px;}
a { color: #006fb3; text-decoration: none; }
a:hover { color: #006fb3; text-decoration: underline;}

/* Sidebar */
#sidebar { width: 312px; float: left; margin-left: 20px;}
#sidebar .widgets { border: 1px solid #ddd; background-color: #FFF; margin-bottom: 50px;
	-webkit-border-radius: 5px; -moz-border-radius: 5px; border-radius: 5px;}
#sidebar .widgets h2 { color: #4d4e54; font-size: 14px; clear: both; margin-bottom: 13px;}
#sidebar .widgets ul li { font-size: 11.5px; }
#sidebar .widgets ul li a { color: #4d4e54; }
#sidebar .widgets .widget-bg { position: absolute; top: -13px; right: 15px; width: 35px; height: 40px; }

/* Follow us list */
#sidebar .follow-us-widget { overflow: hidden; padding-bottom: 35px; border-bottom: 1px solid #ccc; }
#sidebar .follow-us-widget .widget-wrapper { padding: 15px;}
#sidebar ul#follow-list li { float: left; position: relative; margin-right: 17px; zoom: 1; display: inline;}
#sidebar ul#follow-list li:last-child { margin-right: 0;}
#sidebar ul#follow-list li .follow-div { margin:0; padding:0; width: 33px; height: 38px; }
#sidebar ul#follow-list li a { display: block; width: 33px; height: 38px; text-indent: -9999px; background: url('http://blog.linkedin.com/wp-content/themes/linkedin/images/sprite3.png');}
#sidebar ul#follow-list li a#follow-lnkd { background-position: 0 0; }
#sidebar ul#follow-list li a#follow-twtr { background-position: -33px 0; }
#sidebar ul#follow-list li a#follow-fb { background-position: -66px 0; width: 32px; }
#sidebar ul#follow-list li a#follow-flickr { background-position: -130px 0; width: 32px;}
#sidebar ul#follow-list li a#follow-youtube { background-position: -98px 0; width: 32px;}
#sidebar ul#follow-list li a#follow-rss { background-position: -162px 0; width: 32px; }
#sidebar .widgets ul#follow-list li.last { margin-right: 0;}

/* Flickr */
#sidebar .flickr-widget { position: relative; border-bottom: 1px solid #ccc; }
#sidebar .flickr-widget .widget-wrapper { padding: 15px;}
#sidebar .flickr-widget h2 { margin-bottom: 20px; }
#sidebar .flickr-widget .widget-bg { background: url('images/sprite3.png') -267px 0 no-repeat;}
#sidebar #flickr-img-grp { margin-bottom: 10px; overflow:hidden; }
#sidebar #flickr-img-grp .flickr-img { float: left; margin: 0 15px 15px 0; }
	</style>
</head>
<body>
	<div id="sidebar">

		<div class="widgets">
			<div class="follow-us-widget">
				<div class="widget-wrapper">
					<h2>Follow Us Links</h2>

					<ul id="follow-list">
						<li><a id="follow-lnkd"
							href="https://www.linkedin.com/company/linkedin" target="_blank">LinkedIn</a>
						</li>
						<li><a id="follow-twtr" href="http://twitter.com/LinkedIn"
							target="_blank">Twitter</a></li>
						<li><a id="follow-fb"
							href="https://www.facebook.com/LinkedIn" target="_blank">Facebook</a>
						</li>
						<li><a id="follow-youtube"
							href="http://www.youtube.com/user/LinkedIn" target="_blank">YouTube</a>
						</li>
						<li><a id="follow-flickr"
							href="http://www.flickr.com/groups/linkedin/pool/"
							target="_blank">Flickr</a></li>
						<li class="last"><a id="follow-rss"
							href="http://feeds.feedburner.com/LinkedInBlog" target="_blank">RSS</a>
						</li>
					</ul>

				</div>
			</div>

		</div>
	</div>

</body>
</html>

```

## HTML 嵌入图片

```

<img src="data:image/png;base64," />

```

## 页面内容安全

### 禁止鼠标右键

修改 body 标签，加入 onContextMenu="return false" onSelectStart="return false"。

```

<body bgColor="#FFFFFF" onContextMenu="return false" onSelectStart="return false">

```

### 禁止复制剪切 及粘贴

禁止拷贝文字：

```

<input name="textfield" type="text" value="不能复制里面的字" oncopy="return false;" oncut="return false;" onpaste="return false">

```

## html,css 有效性检查 Validation

有效性检查包括

1.  Markup Validation

2.  CSS Validation.

3.  URL Validation.

## 自适应宽度超出截取并显示省略字符

```

<style type="text/css">
.ellipsis {
    white-space: nowrap; 		/*保留文字间的空白*/
    width: 200px;
    overflow: hidden;  			/*超出部分隐藏，*/
    text-overflow: ellipsis; 	/*超出部分显示成... */
    border:1px solid #333;
}
</style>

<div class="ellipsis"> xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx </div>

```

## 第 16 章 Angular

https://angularjs.org/

## Function

```

    $scope.getClass = function(a){                                                                                    |                            <li class="baidu">                                                                       
            return a;                                                                                                 |                                <a href=""></a>                                                                      
    }

```

### ng-bind

```

<div>
  Hello, {{user.name}}
</div>

<div>
  Hello, <span ng-bind="user.name"></span>
</div>

```