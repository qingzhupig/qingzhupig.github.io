---
layout: post
title: "如何卸载pkg安装的软件"
categories: osx
---

> Note: 对于从Apple安装的一些package，可能涉及到文件的更新，不建议手动删除。

#### 前言
昨天在Mountion Lion中安装[`CocosStudio`][1] 2.0.6，其Installer竟然最低只支持OSX 10.9，所以采用查看Installer内容，并手动点击pkg的形式来安装。

一切就绪，怀着探索的心情打开Cocos，并创建一个工程，打开，结果提示我的MBP（Late 2011）显卡不支持`OpenGL 2`，研究一番未果，还是等`CocosStudio`更新。

先卸载了吧。

#### 查看安装纪录

{% highlight bash %}
# 查看安装纪录
$ plutil -p /Library/Receipts/InstallHistory.plist
  13 => {
    "displayName" => "Cocos Studio"
    "displayVersion" => ""
    "date" => 2014-12-27 02:49:29 +0000
    "processName" => "Installer"
    "packageIdentifiers" => [
      0 => "com.ccs.pkg"
      1 => "com.sample.pkg"
    ]
  }
{% endhighlight %}

根据安装纪录，得到安装`CocosStudio`一共安装了`com.ccs.pkg`和`com.sample.pkg`这两个package。


#### 查看安装的文件

{% highlight bash %}
# 查看pkg的安装目录和安装文件
$ pkgutil --pkg-info com.ccs.pkg
package-id: com.ccs.pkg
version: 0
volume: /
location: Applications
install-time: 1419648569
$ pkgutil --only-dirs --files com.ccs.pkg | cut -d'/' -f1-2 | uniq
Cocos
Cocos/Cocos Studio 2.app
Cocos/Cocos.app
$ pkgutil --pkg-info com.sample.pkg
package-id: com.sample.pkg
version: 0
volume: /
location: Library/Application Support/Cocos/CocosStudio2
install-time: 1419648569
$ pkgutil --only-dirs --files com.sample.pkg | cut -d'/' -f1-2 | uniq
Samples
Samples/Demo
{% endhighlight %}

`com.ccs.pkg`将`Cocos`目录安装到`/Applications`下，`com.sample.pkg`将`Samples`目录安装到`/Library/Application Support/Cocos/CocosStudio2`下。


#### 其他查看相关信息的命令
{% highlight bash %}
# 列出所有的packages
$ pkgutil --pkgs
$ cd /private/var/db/receipts
$ ls com.css.pkg* com.sample.pkg*
com.ccs.pkg.bom      com.sample.pkg.bom
com.ccs.pkg.plist    com.sample.pkg.plist
$ plutils -p com.ccs.pkg.plist | grep InstallPrefixPath
  "InstallPrefixPath" => "Applications"
$ lsbom -pf com.ccs.pkg.bom | cut -d'/' -f2 | sort | uniq
.
$ plutil -p com.sample.pkg.plist | grep InstallPrefixPath
  "InstallPrefixPath" => "Library/Application Support/Cocos/CocosStudio2"
$ lsbom -pf com.sample.pkg.bom | cut -d'/' -f2 | uniq
.
Samples
{% endhighlight %}

#### 删除pkg安装的文件
{% highlight bash %}
# 删除pkg安装的文件
$ cd /Applications
$ sudo rm -rf Cocos 
$ cd /Library/Application\ Support/Cocos/CocosStudio2
$ sudo rm -rf Samples
{% endhighlight %}

此时，用`pkgutil --pkgs`仍然可以看到`com.ccs.pkg`和`com.sample.pkg`，执行：

{% highlight bash %}
# 忘记package
$ sudo pkgutil --forget com.ccs.pkg
Forgot package 'com.ccs.pkg' on '/'.
$ sudo pkgutil --forget com.sample.pkg
Forgot package 'com.sample.pkg' on '/'.
{% endhighlight %}

Over

[1]: http://www.cocos2d-x.org/download


