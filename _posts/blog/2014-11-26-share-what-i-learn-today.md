---
layout: default
title: Share What I Learn Today
catgoary: 每天学点新东西
---

# 1. gulp

gulp watch less 遇到错误时终止的解决方法，监听 error ：

```
var gulp = require('gulp');
var gutil = require('gulp-util');
var less = require('gulp-less');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('less', function(){
	var srcLess = '/less';
	var distCss = '/css';

	/* 不编译所有以下划线 `_` 开头的文件或者文件夹 */
	 gulp.src([srcLess+'/**/*.less','!'+srcLess+'/**/_*.less', '!'+srcLess+'/**/_**/**' ])
	    .pipe(sourcemaps.init())
	    .pipe(less({compress: true}).on('error', gutil.log))
	    .pipe(sourcemaps.write('./'))
	    .pipe(gulp.dest(distCss));
});

```

# 2. chrome Auto-reload generated CSS

这货比较麻烦。
1. 必须 watch less 编译，这东西得借助 nodejs 。
2. 把 less 文件加入工作区，并map上。
3. 修改 less 文件， 并点击 css 文件，让它自动刷新。点击这一步骤比较奇葩。