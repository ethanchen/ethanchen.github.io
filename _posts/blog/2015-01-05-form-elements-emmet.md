---
layout: default
title: 表单控件的emmet代码片段
---

```emmet
	fieldset>input[type=text]+input[type=text][disabled]+input[type=text][readonly]
	fieldset>(select>option*2>{$})+(select[disabled]>option*2>{$})
	fieldset>(select[multiple]>option*2>{$})+(select[multiple][disabled]>option*2>{$})
	fieldset>textarea+textarea[disabled]+textarea[readonly]
	fieldset>input[type=checkbox]+input[type=checkbox][disabled]+input[type=checkbox][checked]
	fieldset>input[type=radio]+input[type=radio][disabled]+input[type=radio][checked]
	fieldset>button{button}+input[type=button][value=input]+input[type=submit]+input[type=reset]+.button{div.button}+a.button{a.button}
	fieldset>button[disabled]{button}+input[type=button][disabled][value=input]+input[type=submit][disabled]+input[type=reset][disabled]+div[disabled].button{div.button}+a[disabled].button{a.button}
```

```less

.button() {

} 
.button(hover) {
}
.button(active) {
}
.button(focus) {
}
.button(disabled) {
	color:#ccc;
}


.input {
	box-sizing: border-box;
	background-color: none;
	border:1px solid #ccc;
	padding:5px 10px;
	height: 30px;
	line-height: 30px;
}
.input(hover){
	 
}
.input(focus){
	border:1px solid #A5C7FE;
	outline: 0;
}
.input(select){
	cursor: pointer;
}
.input(textarea){
	height: auto;
}
.input(select-multiple){
	height: auto;
}
.input(disabled){
	cursor: default;
	color:#ccc;
}
.input(readonly){
	cursor: default;
}
 

.checkbox {
	cursor: pointer;
}
.checkbox(focus) {}
.checkbox(disabled) {
	cursor: default;
}
.checkbox(active) {}
 
.radio {
	cursor: pointer;
}
.radio(focus) {}
.radio(disabled) {
	cursor: default;
}
.radio(active) {}


.button,
button,
[type=button],
[type=submit],
[type=reset] {
	& {
		.button;
	}	
	&:hover {
		.button(hover);
	}
	&:active {
		.button(active);
	}
	&:focus {
		.button(focus);
	}
	&[disabled] {
		.button(disabled);
	}
}


input[type=text],
input[type=password] {
	& {
		.input;
	}
	&:hover {
		.input(hover);
	}	
	&:focus {
		.input(focus);
	}
	&[readonly] {
		.input(readonly);
	}
	&[disabled]{
		.input(disabled);
	}
}
textarea {
	& {
		.input;
		.input(textarea);
	}
	&:hover {
		.input(hover);
	}	
	&:focus {
		.input(focus);
	}
	&[readonly] {
		.input(readonly);
	}
	&[disabled]{
		.input(disabled);
	}	
}
 

select {
	& {
		.input;
		.input(select);
	}
	&[multiple] {
		.input(select-multiple);		
	}
	&:hover {
		.input(hover);
	}	
	&:focus {
		.input(focus);
	}
	&[disabled]{
		.input(disabled);
	}
}

input[type=checkbox] {
	& {
		.checkbox;
	}
	&[disabled] {
		.checkbox(disabled);
	}
}
input[type=radio] {
	& {
		.radio;
	}
	&[disabled] {
		.radio(disabled);
	}
}
```
 
