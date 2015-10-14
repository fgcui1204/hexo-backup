title: "gulp 学习"

date: 2015-07-19 20:15:01

tags: 工具 gulp

categories: 工具
----------------

##gulp介绍

Gulp是一个自动化的项目构建工具，也是基于nodeJS环境的,可以通过创建任务（Task）来帮助我们自动完成一些工作流的内容，充分利用了管道（pipe）思想。

他的主要用途编译Jade, Less等文件，压缩CSS,JavaScript和图片等，从而减少客户端对服务Http请求，已达到增强页面的加载速度， 优化服务器带宽压力等等目的。

##gulp安装

Gulp 是基于 Node.js 的，故要首先安装 [Node.js](https://nodejs.org)：[通过nvm安装node教程](https://github.com/creationix/nvm)

-	node安装完成之后通过 下面的命令安装Gulp：

```
npm install -g gulp
```

> -g表示在全局环境安装，以便任何项目都能使用它

-	通过`gulp -v`查看gulp是否安装成功。

-	在自己的项目中安装gulp依赖

```
npm install --save-dev gulp
```

-	在项目跟目录新建gulp的配置文件gulpfile.js。这个文件用来配置我们所需的task, 接下来会具体讲解

```
var gulp = require('gulp')

gulp.task('default', function() {
   console.log('this is the default task')
})
```

-	运行gulp

```
gulp
```

或

```
gulp task_name
```

##gulpfile.js

`gulpfile.js`用来定义我们需要自动化的任务，里面包含了很多task以及依赖关系。

首先给大家看一个task的代码：

```
var gulp = require('gulp');
var uglify = require('gulp-uglify');

gulp.task('minify', function () {
  gulp.src('js/app.js')
    .pipe(uglify())
    .pipe(gulp.dest('build'))
});
```

上面代码中，`gulpfile.js`加载`gulp`和`gulp-uglify`模块之后，使用gulp模块的task方法指定任务minify。 task方法有两个参数，第一个是任务名，第二个是任务函数。

在任务函数中， 使用gulp模块的src方法，指定所要处理的文件，然后使用pipe方法，将上一步的输出转为当前的输入，进行链式处理。 从上面的例子中可以看到，gulp充分使用了“管道”思想，就是一个数据流（stream）：src方法读入文件产生数据流， dest方法将数据流写入文件，中间是一些中间步骤，每一步都对数据流进行 一些处理。

task方法的回调函数使用了两次pipe方法，也就是说做了两种处理。第一种处理是使用gulp-uglify模块，压缩js源码； 第二种处理是使用gulp模块的dest方法，将上一步的输出写入本地文件，这里是build.js（代码中省略了后缀名js）。

执行minify任务时，就在项目目录中执行下面命令就可以了。

```
gulp minify
```

下面给大家介绍常用到的插件，可去[gulp官网](http://gulpjs.com/plugins/)查看所有插件,安装时使用`npm` 安装即可。

-	[gulp-uglify](https://github.com/terinjokes/gulp-uglify) Js压缩插件

-	[gulp-minify-css](https://github.com/murphydanger/gulp-minify-css) Css压缩插件

-	[gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin) 图片压缩插件，支持格式： PNG, JPEG, GIF and SVG

-	[gulp-strip-debug](https://github.com/sindresorhus/gulp-strip-debug) 清除源文件console,debugger代码

-	[gulp-useref](https://github.com/jonkemp/gulp-useref) 合并压缩html文件中的文件

```
// 引入 gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint');
var less = require('gulp-less');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// 检查脚本：Link任务会检查js/目录下得js文件有没有报错或警告。
gulp.task('lint', function() {
    gulp.src('./js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译less：lessCompiler任务会编译public/style/目录下的less文件，并把编译完成的css文件保存到.tmp/style目录中。
gulp.task('lessCompiler', function () {

  gulp.src('public/style/*.less')
    .pipe(less())
    .pipe(gulp.dest('.tmp/style'))

});


// 合并，压缩文件:scripts任务会合并js/目录下得所有得js文件并输出到dist/目录，然后gulp会重命名、压缩合并的文件，也输出到dist/目录。
gulp.task('scripts', function() {
    gulp.src('./js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./dist'));
});

// 默认任务:这时，我们创建了一个基于其他任务的default任务。使用.run()方法关联和运行我们上面定义的任务，使用.watch()方法去监听
指定目录的文件变化，当有文件变化时，会运行回调定义的其他任务。
gulp.task('default', function() {
    gulp.run('lint', 'lessCompiler', 'scripts');

    // 监听文件变化
    gulp.watch('./js/*.js', function() {
        gulp.run('lint', 'sass', 'scripts');
    });
});
```

##模块

### gulpfile.js中一一加载模块

```
var gulp = require('gulp'),
    jshint = require('gulp-jshint'),
    uglify = require('gulp-uglify'),
    concat = require('gulp-concat');

gulp.task('js', function() {
    return gulp.src('js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'))
        .pipe(uglify())
        .pipe(concat('app.js'))
        .pipe(gulp.dest('build'));
});
```

这种一一加载的写法，比较麻烦。使用gulp-load-plugins模块，可以加载package.json文件中所有的gulp模块。

package.json

```
{
    "devDependencies": {
        "gulp-concat": "~2.2.0",
        "gulp-uglify": "~0.2.1",
        "gulp-jshint": "~1.5.1",
        "gulp": "~3.5.6"
    }
}
```

gulpfile.js

```
var gulp = require('gulp'),
    gulpLoadPlugins = require('gulp-load-plugins'),
    plugins = gulpLoadPlugins();

gulp.task('js', function() {
    return gulp.src('js/*.js')
        .pipe(plugins.jshint())
        .pipe(plugins.jshint.reporter('default'))
        .pipe(plugins.uglify())
        .pipe(plugins.concat('app.js'))
        .pipe(gulp.dest('build'));
});
```

###gulp-livereload模块

> 模块用于自动刷新浏览器，反映出源码的最新变化。它除了模块以外，还需要在浏览器中安装插件，用来配合源码变化。

```
var gulp = require('gulp'),
    less = require('gulp-less'),
    livereload = require('gulp-livereload'),
    watch = require('gulp-watch');

gulp.task('less', function() {
    gulp.src('less/*.less')
        .pipe(watch())
        .pipe(less())
        .pipe(gulp.dest('css'))
        .pipe(livereload());
});
```

上面代码监视less文件，一旦编译完成，就自动刷新浏览器。

###gulp.src()

gulp模块的src方法，用于产生数据流。它的参数表示所要处理的文件，一般有以下几种形式。

`js/app.js`：指定确切的文件名。

`js/*.js`：某个目录所有后缀名为js的文件。

`js/**/*.js`：某个目录及其所有子目录中的所有后缀名为js的文件。

`!js/app.js`：除了js/app.js以外的所有文件。

`*.+(js|css)`：匹配项目根目录下，所有后缀名为js或css的文件。

`['js/**/*.js', '!js/**/*.min.js']`：还可以是一个数组，用来指定多个成员。

###gulp.task()

gulp模块的task方法，用于定义具体的任务。它的第一个参数是任务名，第二个参数是任务函数。 task方法还可以指定按顺序运行的一组任务：

```
gulp.task('css', function() {
    console.log('css...');
});

gulp.task('js', function() {
    console.log('js...');
});

gulp.task('imgs', function() {
    console.log('imgs...');
});

gulp.task('build', ['css', 'js', 'imgs']);
```

> 注意，由于每个任务都是异步调用，所以没有办法保证js任务的开始运行的时间，正是css任务运行结束。

严格的按顺序执行：

```
gulp.task('greet', function() {
    console.log('Hello world!');
});

gulp.task('css', ['greet'], function() {
    console.log('css...');
});

gulp.task('js', ['css'], function() {
    console.log('js...');
});

gulp.task('build', ['js']);

```

###gulp.watch()

gulp模块的watch方法，用于指定需要监视的文件。一旦这些文件发生变动，就运行指定任务。

```
gulp.task('watch', function() {
    gulp.watch('templates/*.tmpl.html', ['build']);
});
```

> 上面代码指定，一旦templates目录中的模板文件发生变化，就运行build任务

参考：

[利用Gulp优化部署Web项目](http://imziv.com/blog/article/read.htm?id=60)

[Gulp：任务自动管理工具](http://javascript.ruanyifeng.com/tool/gulp.html)
