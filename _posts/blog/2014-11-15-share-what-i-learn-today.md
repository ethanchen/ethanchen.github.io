---
layout: default
title: Share What I Learn Tody
catgoary: 每天学点新东西
---

# SSH 上传文件

`scp -P 2222 local/file.tar.gz remote@host.com:~/public_html`

注意端口号参数是大写P，上传是 local -> remote 下载是 remote -> local


# git 导出项目

`git archive --format zip --output ../project_bak.zip master`

翻译：把master分支压缩到父目录


# linux 解压

`tar -xzvf file.tar.gz`

tar 表示 Tape archive 打包，x是extract解压，z是gzip，v是verbose显示解压出来的文件，f是文件，后面接文件名


# vim 移动光标到行末

行末 `g$`, 行首 `g^`, 搜索 `/keyword`