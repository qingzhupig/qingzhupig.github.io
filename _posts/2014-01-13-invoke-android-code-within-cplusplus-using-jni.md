---
layout: post
title: "从CPP代码通过JNI调用Java代码"
categories: development
---

> Important: 
> 
> 尽量不要把 `JNI`写在新线程里，`Cocos2d-x`的`JniHelper`类没有做相关的处理，会找不到你要用的方法。

关于新线程里使用`JNI`可以参考：

- [JNI Tips - ANdroid Developers][10]
- Android multithread in C/C++ to call JNI [1][11] [2][12]

#### 前言
在[Mac下用命令行来移植Cocos2dx Android项目][1]中已经描述了如何将Cocos2dx游戏编译、运行到Android设备上，但是现实开发中，通常会有一些和平台相关的功能或者调用，这个时候需要额外的为Android平台做些处理。

本篇主要处理2个问题:`打开一个外部URL`和`获取OpenUDID`； 这里以处理OpenUDID为例，打开URLs类似处理即可。

#### 相关文章和资源

- [Open URLs in external browser][2]
- [OpenUDID on Android][3]

#### 下载文件和设置环境
从上面的链接中下载OpenUDID，解压之后，查看java文件源码得到包名(com.OpenUDID)，根据包名组织文件夹结构，如下图：

``` bash
$ tree src
src
├── com
│   └── cocos2dx
│       └── test
│           ├── Test.Original.java
│           └── Test.java
└── org
    └── OpenUDID
        ├── OpenUDID_manager.java
        └── OpenUDID_service.java

5 directories, 4 files
```

使用OpenUDID还需要在Android的AndroidManifest.xml中做些设置，具体参考OpenUDID的Readme。

#### 增加Java代码
显然，直接修改cocos2d-x自带的文件不是一个好的习惯，所以这里我们修改src/com/cocos2dx/test/Test.java，直接上diff -u 的输出:

{% highlight diff linenos %}
$ diff -u src/com/cocos2dx/test/Test.Original.java src/com/cocos2dx/test/Test.java
--- src/com/cocos2dx/test/Test.Original.java	2014-01-13 17:33:24.000000000 +0800
+++ src/com/cocos2dx/test/Test.java	2014-01-13 17:27:06.000000000 +0800
@@ -23,6 +23,7 @@
 ****************************************************************************/
 package com.cocos2dx.test;
 
+import org.OpenUDID.OpenUDID_manager;
 import org.cocos2dx.lib.Cocos2dxActivity;
 import org.cocos2dx.lib.Cocos2dxGLSurfaceView;
 
@@ -32,6 +33,7 @@
 	
     protected void onCreate(Bundle savedInstanceState){
 		super.onCreate(savedInstanceState);	
+		OpenUDID_manager.sync(this);
 	}
 
     public Cocos2dxGLSurfaceView onCreateView() {
@@ -45,4 +47,12 @@
     static {
         System.loadLibrary("cocos2dcpp");
     }     
+
+	public static String getOpenUDID() {
+		if ( OpenUDID_manager.isInitialized() ) {
+			return OpenUDID_manager.getOpenUDID();
+		}
+
+		return null;
+	}
 }
{% endhighlight %}
	
忘记diff的含义也不要紧，花几分钟[读懂diff][4]。

#### 编写C++方法，调用getOpenUDID
无码无真相，这里提供一个帮助方法，当C++中要获取OpenUDID时，调用该方法即可。

{% highlight cpp %}
#include "jni/JNIHelper.h"

std::string Helper::getOpenUDID()
{
	std::string udid;

	JniMethodInfo info;
	if ( JniHelper::getStaticMethodInfo(info, "com/cocos2dx/test/Test", "getOpenUDID", "()Ljava/lang/String;") ) {
		jstring udidstring = (jstring)info.env->CallStaticObjectMethod(info.classID, info.methodID);
		if ( udidstring )
			udid = JniHelper::jstring2string(udidstring);
	}

	return udid;
}
{% endhighlight %}

得解释一下，`com/cocos2dx/test/Test`是方法所在的类名（包含包名）；`getOpenUDID`是要调用的静态方法名；`()Ljava/lang/String;`则是改方法的签名。

签名有自己的规则，`()Ljava/lang/String;`表示该方法没有参数，返回一个String。通常可以写个测试类，将需要获得签名的方法写入，利用`javap`工具来自动获取签名，如：

{% highlight bash %}
$ cat TestSignature.java
class TestSignature {
	public static String getOpenUDID()
	{
		return null;
	}
}
$ javac TestSignature.java
$ javap -s TestSignature
Compiled from "TestSignature.java"
class TestSignature extends java.lang.Object{
TestSignature();
  Signature: ()V
public static java.lang.String getOpenUDID();
  Signature: ()Ljava/lang/String;
}
{% endhighlight %}
	
So easy! 再用不用担心写错签名了！

[1]: {% post_url 2014-01-10-port-cocos2dx-game-to-android-from-command-line-on-mac %}
[2]: http://www.cocos2d-x.org/forums/6/topics/11290
[3]: https://github.com/vieux/OpenUDID
[4]: http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html

[10]: http://developer.android.com/training/articles/perf-jni.html
[11]: http://blog.csdn.net/wu4long/article/details/17756419
[12]: http://blog.csdn.net/wu4long/article/details/17757433
