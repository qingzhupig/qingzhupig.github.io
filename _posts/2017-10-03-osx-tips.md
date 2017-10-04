---
layout: post
title: "OSX小技巧"
categories: osx
---

## 取消`iOS`设备的信任

当`iOS`设备第一次连接到`OS X`的时候，`OS X`会弹出“信任此电脑”的提示，通常会点击“同意”。

当想取消信任时，

``` bash
# rm -rf /private/var/db/lockdown/
```
