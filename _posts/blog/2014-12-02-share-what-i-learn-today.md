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

