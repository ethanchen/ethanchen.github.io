---
layout: default
title: Share What I Learn Today
catgoary: 每天学点新东西
---

“有限状态机”这东西我也不是很清楚，就是大概了解一下概念，一个交互控件可以这么描述：

一个控件可能有多种状态的，但是任何时候都只会处在一种状态下，且状态是可以切换。


我们来设计一个实时编辑控件，以下是交互过程：

1. 一开始是普通文本
2. 点击文本的时候，变成一个文本框、确认按钮和取消按钮
3. 点击确认按钮，提交文本框内容，并变成新的普通文本
4. 点击取消按钮，变成原来的普通文本

当拿到这么一个需求的时候，可能一开始就会写下一堆 click 绑定事件，当交互变成更加复杂的时候，代码渐渐变得杂乱无章。

但如果我们换个角度来思考：

这个控件有两种状态，初始化是阅读状态，点击的时候就变成编辑状态，提交或者取消后又变成阅读状态。

于是用程序语言可以这么描述：

```
editInPlace = {
	states:{'view', 'editing'},
	current:'view',
	events:{
		edit: {from:'view', to:'editing'},
		submit: {from:'editing', to:'view'},
		cancel: {from:'editing', to:'view'}
	},
	edit: function() {
		// if current === editing return false
		// hide text and show inputs
	},
	submit: function() {},
	cancel: function() {}
	
}
$(text).click(editInPlace.edit);
$(input).click(function(){
	// if press enter
	//    editInPlace.submit
	// if press esc
	//    editInPlace.cancel
});
$(btnSubmit).click(editInPlace.submit);
$(btnCancel).click(editInPlace.cancel);
```

以上就描述了整个交互过程，是不是清晰很多。

之前做程序设计都是用户行为事件驱动，导致在设计时的思路是：

“当用户点击按钮的时候执行一系列操作”

而对于控件来说却是相反的，从控件角度来说，就变成这么思考：

“控件有几种状态，提供几种行为事件，什么时候转变状态”

首先考虑是状态，而不是什么时候转变。

做交互设计是要从用户角度出发，做程序设计还是得从程序角度出发。



