---
layout: post
title: "Git小技巧"
categories: git
---

[Git](https://github.com/git)的使用技巧非常多，这里记录一下曾经使用过的一些技巧。

## 查看某人提交的代码

``` bash
$ git log --author=xxx
```

如果想在外部工具中查看，比如`perforce`中，首先配置好[p4diff][1]，然后执行

``` bash
$ git log -p --author=xxx --ext-diff
```

[1]: {% post_url 2017-03-03-install-and-setup-p4merge %}
