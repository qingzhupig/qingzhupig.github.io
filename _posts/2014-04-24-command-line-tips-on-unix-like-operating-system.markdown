---
layout: post
title: "Unix类系统的一些命令行Tips"
date: 2014-04-24 13:35:04 +0800
categories: commandline, tips
---

#### Tips for bash shell

> 获取当前脚本的目录

``` bash
# 可处理目录中有空格的情况
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}")" && pwd )"
# 或者
DIR=$( dirname "$0" )
```

> 复制目录下所有包含特定文本的文件

``` bash
# 将这些文件拷贝一份
$ grep -iR 'pattern' . | cut -d: -f1 | cut -d'/' -f2- | xargs -I{} rsync -avR {} copypath
```

> 替换目录中的特定文本

``` bash
# 比如将当前目录中所有的iPhone 替换为iPhone 4S
$ find . -type f -print0 | xargs -0 perl -pi -e 's/iPhone/iPhone 4S/g'
```

