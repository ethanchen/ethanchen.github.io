---
layout: default
title: 用 gulp 做静态资源版本控制
---

目录结构 

```
src
  |-- css/style.css
  |-- img/bg.jpg
  |-- index.html
```

gulfile.js

```javascript
var gulp = require('gulp');
var revall = require('gulp-rev-all');

gulp.task('default', function () {
	return gulp.src('src/**')
		.pipe(revall({ ignore: ['.html'] }))
		.pipe(gulp.dest('dist'));
}); 
```

output

```
dist
  |-- css/style.fcd04d6e.css
  |-- img/bg.bf34f965.jpg
  |-- index.html
```

而 html 中引用的 css 和 css 中引用的 img 也会相应的改变。
src 和 dist 中 index.html 都可以正常访问，很酷吧