---
layout: post
title: 安装和配置p4merge
date: 2017-03-03 20:21:10 +0800
categories: 
---

#### Updated: 2020-03-04

* `Mac OS X`下需要使用`CMD + Q`退出上一个文件，才能查看下一个文件
* `Mac OS X`下，如果有`rename`的变更时，会在对比到该文件时报错退出，此时可以使用`git diff --no-renames`来跳过`rename`
* 当高亮显示的变更错位时，选择一个合适的字体会好很多

`p4merge`是一个图形化的`diff`与`merge`工具，界面简洁，我很喜欢。这里记录一下安装`p4merge`的过程、设置`git`使用`p4merge`。

> 本文适合Debian Linux和Mac OS X

到[官方网站](https://www.perforce.com/downloads/helix#clients), 找到`HELIX P4V: VISUAL CLIENT`, 选择合适的版本下载，得到`p4v.tgz`文件

#### 安装

``` bash
$ sudo mkdir /usr/local/p4v
$ sudo tar --strip-components=1 -xvzf p4v.tgz -C /usr/local/pv4
$ sudo chown -R root:staff /usr/local/p4v
```

#### 创建实际使用的shell脚本

``` bash
$ cat /usr/local/bin/p4merge
#!/usr/bin/env bash

CMD="/usr/local/p4v/bin/p4merge"
${CMD} $*
$ cat /usr/local/bin/p4diff
#!/usr/bin/env bash

CMD="/usr/local/p4v/bin/p4merge"
${CMD} "$2" "$5"
```

#### 设置`git`
``` bash
$ git config --global merge.tool p4merge
$ git config --global mergetool.p4merge.cmd 'p4merge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"'
$ git config --global mergetool.trustExitCode false
$ git config --global diff.external p4diff
```
