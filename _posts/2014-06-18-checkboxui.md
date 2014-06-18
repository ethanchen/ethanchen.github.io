---
layout: post
title: 改变 checkbox 的 UI
---
## {{ page.title }}
{{ page.date | date_to_string }}

## 想法一：给 [type=checkbox] 换背景

勾选状态可以通过伪类和js（for ie8-）解决

```
[type=checkbox] {
	background:url(xxx.png) no-repeat;
}
[type=checkbox].checked, 
[type=checkbox]:checked {
	background-position: 0 -20px;
}
```

但是问题是：如何去掉原生的 checkbox 样式？

```
-webkit-appearance:none;
```

firefox/IE 无效，就算是 `-moz-appearance:none;` 也去不掉丑陋的框框

## 想法二：隐藏原生 checkbox ，增加一个模拟的 checkbox

使用 [type=checkbox]:after 生成模拟元素

悲催地发现 firefox 在 checkbox 情况下无效

使用 js 生成模拟元素

另一个问题产生了：如果让原生 checkbox 和模拟 checkbox 同步勾选状态？

一般情况下通过 change 或者 click 等事件监听是没问题的。但是如果通过 js 的调用方式，例如：`input.checked = true` 这样，就无法监听得到了。所幸的是 ie 可以使用 propertychange 来捕获。而其他浏览器，有同事说可以使用“被动检测”，我稍微试了一下，没成功，而且这东西也只有比较高版本的浏览器才支持，所以暂且放弃。

## 综合方案：针对不同的浏览器指定方案

1. chrome, safari 直接原生css替换背景
	```
	[type=checkbox] {
		background:url(xxx.png) no-repeat;
		-webkit-appearance:none; /* 隐藏原生 checkbox */
	}
	[type=checkbox]:checked {
		background-position: 0 -20px;
	}	
	```
2. firefox 仅通过 js 生成模拟元素, 状态切换使用伪类
	```
	[type=checkbox] {
		 opacity: 0  !important; /* 两个作用，第一是隐藏checkbox，第二是提高checkbox层级，可以点到 */
		 filter: alpha(opacity=0); 
		 margin-right: -15px  !important; /* 由于 opacity 的原因，可以覆盖到旁边 uicheckbox 上面，可以被点到 */
	}
	.uicheckbox {
		background:url(xxx.png) no-repeat;
		width:15px;
		height:15px;
	}
	[type=checkbox]:checked + .uicheckbox {
		background-position: 0 -20px;
	}	
	```
3. ie 通过 propertychange 改变模拟元素的 class
	```
	.uicheckbox.checked{ background-position: -20px -40px; }
	```

radio 元素如法炮制，但是注意：同个 form 里面的 name 相同所有的 radio, 也就是 radio 组，ie 需要同步状态

还有其他可能需要注意的：

1. 可访问性，键盘控制
2. label 包裹 checkbox ， 没有 label 的情况
3. 动态生成 checkbox 的情况（目前对于 ie 和 firefox 只能再次初始化）
