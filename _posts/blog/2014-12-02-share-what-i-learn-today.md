---
layout: default
title: Share What I Learn Today
catgoary: 每天学点新东西
--- 

# less 不编译代码片段的 [e 方法](http://lesscss.org/functions/#string-functions-e)

比如遇到ie的filter或者hack等等可以使用 

```
filter: e("ms:alwaysHasItsOwnSyntax.For.Stuff()");
```
输出是
```
filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
```

项目中遇到一些不能通过编译的规则：

- 带符号和`\9`的，如 `top:-11px \9;` 只能改成 `top:e('-11px \9');`

