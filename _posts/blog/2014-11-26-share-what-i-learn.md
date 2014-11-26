---
layout: default
title: Share What I Learn 2014-11-26 
---

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

