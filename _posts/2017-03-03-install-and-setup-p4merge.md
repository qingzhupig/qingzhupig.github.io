---
layout: post
title: 安装和配置p4merge
date: 2017-03-03 20:21:10 +0800
categories: 
---

`p4merge`是一个图形化的`diff`与`merge`工具，界面简洁，我很喜欢。这里记录一下安装`p4merge`的过程、设置`git`使用`p4merge`。

#### Debian Linux下的安装和配置

到[官方网站](https://www.perforce.com/downloads/helix#clients)下载，得到`p4v.tgz`文件

*安装*

``` bash
$ sudo mkdir /usr/local/p4v
$ sudo tar --strip-components=1 -xvzf p4v.tgz -C /usr/local/pv4
$ sudo chown -R root:staff /usr/local/p4v
```

*创建实际使用的shell脚本*

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

#### Mac OS X下的安装和配置

**TODO**

#### 设置`git`
``` bash
$ git config --global merge.tool p4merge
$ git config --global mergetool.p4merge.cmd 'p4merge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"'
$ git config --global mergetool.trustExitCode false
$ git config --global diff.external p4diff
```
