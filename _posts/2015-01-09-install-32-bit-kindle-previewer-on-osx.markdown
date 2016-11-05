---
layout: post
title: "安装32位的Kindle Previewer"
date: 2015-01-09 22:43:39 +0800
categories: osx
---

#### 前言
最近到处找`mobi`格式的E文技术书，这样就可以直接发送到Kindle设备，可以随时学习了。 但是`mobi`格式的书还是比较少的，能找到的大多是`PDF`或者`EPUB`格式的，`PDF`在Kindle上表现极差，即便使用Amazon的服务转换成`mobi`也是不可以接受的。

对于`EPUB`格式，Amazon官方提供了`kindlegen`和`kindle-previewer`来转换和查看其内容，所以搞起。

#### 安装

{% highlight bash %}
# 可能需要用代理
$ brew cask install kindlegen kindle-previewer
{% endhighlight %}

安装完得到`kindlegen`命令和`Kindle Previewer`的App。

试着用了一下，发现`Kindle Previewer`启动是一闪，然后就退出了。Amazon只提供32位版本的App，我的系统上只装有的`Java 8`, 不支持32位，所以自然会无法运行。

下载并安装[`JavaForOSX2014-001`][1]。

{% highlight bash %}
# Kindle Previewer需要32-bit JVM
# 安装前系统支持的Java版本
$ /usr/libexec/java_home -V
Matching Java Virtual Machines (1):
    1.8.0_25, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_25.jdk/Contents/Home
# 进入到App内部，用命令行启动，得到错误提示
$ cd ~/Applications/Kindle\ Previewer.app/Contents/MacOS
$ ./Launcher
Error: This Java instance does not support a 32-bit JVM.
Please install the desired version.
# 安装后系统支持的Java版本
$ /usr/libexec/home_java -V
Matching Java Virtual Machines (3):
    1.8.0_25, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_25.jdk/Contents/Home
    1.6.0_65-b14-466.1, x86_64:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
    1.6.0_65-b14-466.1, i386:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
{% endhighlight %}

既然已经有32位的支持了，设置环境变量JAVA_HOME即可。

{% highlight bash %}
# 设置正确的JAVA_HOME变量来运行App
$ grep JAVA_HOME Launcher
export JAVA_HOME=`/usr/libexec/java_home -v 1.6`
{% endhighlight %}

搞定，收工！

[1]: http://support.apple.com/kb/DL1572?viewlocale=en_US&locale=en_US

