---
layout: post
title: "调试Cocos2dx Android项目"
date: 2014-01-16 22:54:59 +0800
categories: development
---

#### 前言
前面的[Mac下用命令行将Cocos2dx游戏移植到Android平台][1]和[从CPP代码通过JNI调用Java代码][2]两篇文章，已经把移植工作的近90%描述完成，接下来则是最重要的10%，也可能是最麻烦的部分。

没错，调试！ 尤其是可恶的`crash`！！！

本文记录有限的一些调试经验，供参考。

#### 看待Android的调试
已经不止一次遇到这种情况了，在`iOS`平台上跑得好好的游戏，为什么到`Android`里就各种`Bug`？

更惨的是`Android`下无法从源码级别来进行调试，只好不断重复`CCLOG`、编译、烧机和测试，不仅难以定位问题，而且浪费了大量时间。

> 似乎听说现在可以调试`Native Application`了，坐等好消息。

个人认为，上面的这些问题通常都是在`XCode`里编码不严谨、资源管理不佳等原因造成的，因此严格要求自己，处理好代码中的资源管理以及警告信息，将会给移植工作省下很大的麻烦。

言归正题，准备调试吧！

#### 调试准备
设置`Android.mk`和`Application.mk`里的编译选项，不阻止任何的警告信息，即去掉任何`-W`开头的设置，在编译的过程中把所有的警告信息都处理掉。

如有必要，在`Application.mk`中为C和CPP设置`no-stack-protector`编译选项：
``` bash 去除栈保护机制
$ grep no-stack-protector jni/Application.mk
# flags for both c and cpp source code
APP_CFLAGS := -fno-stack-protector
```

#### 开始调试
当遇到`Bug`尤其`crash`的时候，推荐的做法是，先观察`adb logcat`的输出，尝试定位问题。

查看在关键操作调用前后的一些打印输出，借此定位`Bug`大致发生的位置， 然后通过分析源代码来尝试找到`Bug`。

很多时候，`adb logcat`的一些栈信息可读性不高，给问题的定位带来一些难度， 此时可以暂时去掉栈保护机制，使用`ndk-stack`命令将对应的栈信息转化为可读的函数调用信息。
``` bash 使用ndk-stack查看logcat信息
adb -d logcat | ndk-stack -sym obj/local/armeabi/
```

以上的调试方式虽然效率不高，但是简单直接，也算是可行的方案。 或许将来会用到`ndk-gdb`，届时再来更新。

#### 常见问题及解决

> 点击CCScrollView里的元素时不响应点击事件

在`Android`上，当你按下触发一个`Touch`事件的时候，通常会伴随着发生`Move`事件，因此一个有效的解决办法是设置一个阀值。

当移动的距离绝对值低于阀值的时候，按点击事件来处理， 否则按照滑动事件来处理。

> `JniHelper`找不到要调用的`Java`方法

参考[从CPP代码通过JNI调用Java代码][2]


#### 恢复编译选项
当调试完毕之后，删除禁用栈保护机制的配置选项。

[1]: {% post_url 2014-01-10-port-cocos2dx-game-to-android-from-command-line-on-mac %}
[2]: {% post_url 2014-01-13-invoke-android-code-within-cplusplus-using-jni %}

