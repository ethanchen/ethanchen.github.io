---
layout: default
title: ie8下iconfont不展现的解决方法
---

在ie8（正式ie8浏览器，而非ie8文档模式）下发现iconfont不展现，原因是使用了类似 `.icon-bug:before { content: "\E001" }` 伪类作为图标展示。

- 解决方案一，把字符 `\E001` 转为html实体编码 `&#xe001;` 放到html里面
- 解决方案二，[在 document ready 的时候触发一下伪类重绘](http://stackoverflow.com/questions/9809351/ie8-css-font-face-fonts-only-working-for-before-content-on-over-and-sometimes/10557782#10557782)

当然，方案二是比较合理，且适合维护的。
