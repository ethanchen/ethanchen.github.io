---
layout: default
title: conEmu 和 Putty 的整合
---

# 整合

windows 的 cmd 很弱，于是有了 conEmu，这货可以整合 Putty。

下载 conEmu 和 putty。

我下载的 conEmu 已经自带了 {Putty} 这个 task 了，如果没有的话，新建一个 task （win+alt+T 或者右上角的加号下拉菜单里）取名并在多行文本框里贴入

```
"D:\Program Files\ConEmu\putty.exe" -new_console 
```

然后运行该 task 就会以 tab 的方式运行 putty。

# putty 配置

putty 会使用 session 来保存你的配置，所以修改了配置（如 proxy）之后，记得回到 session 项 save 一下。

# putty 颜色

[putty-colors-solarized](https://github.com/altercation/solarized/tree/master/putty-colors-solarized) 

下载 .reg 并运行，打开 putty 会发现多了一个 session 叫 Solarized Dark， 你可以 load 一下获得颜色并改动配置作为自己的 session。