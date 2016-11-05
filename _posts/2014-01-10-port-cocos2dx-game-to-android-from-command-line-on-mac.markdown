---
layout: post
title: "Mac下用命令行将Cocos2dx游戏移植到Android平台"
date: 2014-01-10 16:49:57 +0800
categories: development
---

> 推荐阅读

- [Android Develop Workflow][1]

#### 前言
这两天将一款之前用cocos2dx做的游戏移植到android上，也算是有点小收获和小心得，整理记录一下，方便日后回顾。

#### 安装命令行工具并设置环境变量

``` bash
# install brew
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
# install android-sdk and android-ndk and ant
$ brew install android-sdk android-ndk ant
# set up environment
$ grep android ~/.bash_profile
export ANDROID_HOME=/usr/local/opt/android-sdk
export NDK_ROOT=/usr/local/opt/android-ndk
```

#### 安装Google Android开发包
``` bash
$ android
```

从打开的Android SDK Manager里安装Platform-tools、Build-tools和Android 2.2以及Android 4.4.2的开发包

{% highlight bash %}
# Google Android SDK下载提速, 添加的google android sdk源的hosts
$ grep google /etc/hosts
203.208.46.146 dl.google.com
203.208.46.146 dl-ssl.google.com
# 设置Android SDK Manager
勾选Android SDK Manager-->Preferences...->Force https://... sources to be fetched using http://
{% endhighlight %}

#### 设置cocos2dx环境变量并创建测试项目
这里用的是v2.2的cocos2dx

``` bash
$ grep cocos2dx ~/.bash_profile
export COCOS2DX_ROOT=${HOME}/Development/cocos2dx
$ cd $COCOS2DX_ROOT/tools/project-creator
$ python create_project.py -project Test -package com.cocos2dx.test -language cpp
$ cd ../../projects/Test/proj.android
```

#### 编译cocos2dx项目
这里通过模版提供的build_native.sh脚本，调用ndk-build工具来编译cocos2dx项目，得到动态库:
``` bash
$ sh build_native.sh
```

#### 用ant打包成apk
设置一下该android项目，产生正确的配置文件，然后打测试包

``` bash
$ android update project -p . [-t 1] [-l ../../../cocos2dx/platform/android/java]
$ ant debug
```

如果编译过程中，有类似如下提示：

``` bash
cocos2dx/platform/android/java/build.xml:46: sdk.dir is missing.
```

则需要更新Cocos2dx自带的android项目，使其正确配置sdk.dir

``` bash
$ pushd ${APP_ROOT}/libs/cocos2dx/platform/android/java
$ android update project -p .
$ popd
```

#### 设置Android设备
将Android设备连接到Mac上，`设置`--`开发者选项`--`USB调试`
{% highlight bash %}
# 查看联机设备
$ adb devices
List of devices attached 
3230224cd7eb105b	device
{% endhighlight %}

#### 安装、运行和调试app
这里说的调试其实只是在命令行下现实logcat的输出:-)

```
$ adb -d install -r bin/Test-debug.apk
$ adb shell am start -n com.cocos2dx.test/com.cocos2dx.test.Test
$ adb -d logcat | grep cocos2d
```

#### 使用Makefile简化命令
哈哈，make一知半解，写的直接而丑陋，好在能用
{% gist df4722057d0ac255f6b5 %}


[1]: http://developer.android.com/tools/workflow/index.html

