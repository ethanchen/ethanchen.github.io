---
layout:default
title: dom Range 和 textRange
---

# dom Range 和 textRange

## 两个概念 selection 和 range

selection  就是选区，就是鼠标画上蓝色的区域，理论上是可以包含多个区域，但一般浏览器都不支持选择多个不同区域的文本。
range      区域，在 ie 中是 textRange 基于文本的操作，标准的 range 是基于节点和文本的操作。 区域跟选区是有区别的，区域不一定是被蓝色包围的选区。但我们一般会去让浏览器去执行选中相应的区域。

认识了 selection 和 range 的联系和区别就比较好看代码了。


## 基本思路

如果我们要用代码选中页面上的某个区域，一般的思路是这样:

1. 创建一个 range
2. 改变 range 的范围
3. 让这个 range 被选中


## 旧IE标准 TextRange

ok，按这个思路先说说 IE ：

```
var node = document.getElementById('xxx');
var r = document.selection.createRange(); // {1} 获得当前选取的 TextRange 区域，由 selection 创建
r.moveToElementText(node); // {2}把区域挪到节点上
r.moveStart('character', 1) // 区域的起始位置往后挪一个字符
r.moveEnd('character', -1) // 区域的结束位置往前挪一个字符，注意，往前是负数
r.select(); // 让这个区域选中
```
其实{1}{2}两句可以合并成一句 `var r = node.creatTextRange();` 。但要注意这里有个 Text 字样。


## W3C标准 Range

再来说一下w3c的标准：
```
var s = window.getSelection(); // selection 是属于 window 的
var r = document.createRange(); // 由 document 来创建区域
r.setStart(node,0); // 注意这里的 node 如果是 textNode, 第二个参数表示字符，如果不是，第二个参数表示子节点
r.setEnd(node,1);
s.removeAllRanges(); // 把原选区中的区域们都抛弃
s.addRange(r); // 执行选中
```

## 值得留意的方法

### 把区域变成光标

textRange.collapse(isToStart)

一般设置光标的时候有用，把选区的开头和结尾设置成一样就是光标啦。这个方法默认true，即是让结束位置**一直跟随**开始位置（除非重新设置结束位置）。

Range 也有类似的方法。

### 书签打点

```
var bm = textRange.getBookmark() 
// ... do something
textRange.moveToBookmark(bm)
textRange.select()
```

可以把当前选中的区域加个书签，处理其他事之后再选择回来。常用于对刚刚粘贴内容的处理。

Range 的书签呢？ 
```
var s = window.getSelection()
var bm = s.getRangeAt(0)
// ... do something
s.removeAllRanges()
s.addRange(bm)
````

### inputElement.setSelectionRange(0,2)

在w3c标准中，如果要选择 input 类型元素的文本，那么 range.setStart 方法是无效，只能使用这个方法来选择。在 firefox 上还要加上一个 inputElement.focus() 来触发选中。

注意： 在 contenteditable 中的 inputElement 是无法 focus 里面的文本，所以 ff 这里会有问题。



## 关于粘贴的

监听 paste ，标准浏览器会有一个 clipboard 事件，如 `event.clipboardData.getData('text/plain')`

由于 ie 操作剪切板会有弹框，体验很不好。如果要操作剪切板的内容，可以让内容贴到其他地方，操作之后，在贴回来。
在ie中需要监听 beforepaste ， 而其他浏览器是 paste 。

在粘贴的时候：

1. 记录当前光标位置 bookmark
2. 创建一个node并让它成为选区，这样粘贴的是会贴到它里面去
3. 粘贴之后，操作node，得到 txt
4. 取回 bookmark 选中它，并 document.exceCommand('insetHTML',false, txt);

注：exceCommand 可以对**当前选中的区域**进行操作



## 一个富文本框内粘贴纯文本的例子

```
/* pasteAsPlainText */
var self = {};
self.doc = document;
self.win = window;

if (document.selection) { // ie

	$(self.doc.body).bind('beforepaste', function(e){ // ie paste 事件在 body 上
		var r = self.doc.selection.createRange();
    	var bookmark = r.getBookmark();
		var dom = $('<textarea id="editor-plaintext-helper" style="display:none;">&nbsp;</textarea>').appendTo(self.doc.body)[0]; // 必须有字符，不然选不到
		r.moveToElementText(dom); 
		r.moveStart('character',0); 
		r.moveEnd('character',0);  // 当前选区的结束位置往后移动x个字符
		r.select(); 
		setTimeout(function(){ // 等待粘贴
			var d = self.doc.getElementById('editor-plaintext-helper');
			var txt = d.value; 
			txt = $('<div/>').text(txt).html();
			txt = txt.replace(/\n/g,'<br/>');
			$(d).remove();
    		if (bookmark) {
	    		r.moveToBookmark(bookmark);
	    		r.select();
	    		bookmark = null;
	    		self.insertHtml(txt);	
    		}

		},0);
	});


} else if (window.getSelection) { 

	$(self.doc.body).bind('paste', function(e){
		if (e.originalEvent.clipboardData) {

			e.preventDefault();
			var txt = e.originalEvent.clipboardData.getData('text/plain');
			txt = $('<div/>').text(txt).html();
			txt = txt.replace(/\n/g,'<br/>');
			self.doc.exceCommand('insertHTML',false,txt);

		} else { // 备用方案，由于在firefox 下 textarea 无法聚焦，所以该方案对 ff 无效

			var textarea, bookmark, r, s;
			s = self.win.getSelection();
			r = self.doc.createRange(); 
			bookmark = s.getRangeAt(0);
			textarea = $('<textarea id="editor-plaintext-helper" style="height:0!important;width:0!important:border:0!important;padding:0;overflow:hidden;opacity:0;">&nbsp;</textarea>').appendTo(self.doc.body)[0];   // display:none 会导致无法选中
			textarea.setSelectionRange(0,1); 
			textarea.focus();
			setTimeout(function(){
			 	var d = self.doc.getElementById('editor-plaintext-helper');
				var txt = d.value;
				txt = $('<div/>').text(txt).html();
				txt = txt.replace(/\n/g,'<br/>');
				$(d).remove();
				s.removeAllRanges();
			 	s.addRange(bookmark); 
			 	self.doc.exceCommand('insertHTML',false,txt);
			},0);	

		}
	});

}  
```






