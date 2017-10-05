---
layout: post
title: "逆向分析Navicat for MySQL"
categories: reverse, osx
---

## 申明

本文仅从学习和研究的视角来分析`Navicat for MySQL`，不提供最终可运行的具体文件。

## 源起

[Navicat for MySQL](https://www.navicat.com/en/products/navicat-for-mysql)是我比较喜欢用的一个数据库工具，平时工作中用的是`Linux`版本的，有14天的试用期，试用期结束后，删掉所有配置，重新试用o_o

没办法，`Navicat for MySQL`的`License`太贵了，我等屌丝只好勤快一点儿。

但是在我的`Macbook Pro`上，试用期到了之后，就一直不能再用了，所以我换用一个开源的产品[Sequel Pro](https://www.sequelpro.com/)。

假期正好有点空闲，所以抱着学习的态度，研究一下`Navicat for MySQL`...

## 环境

* Mac OS X 10.12
* [Navicat for MySQL 12.0.14](http://download.navicat.com/download/navicat120_mysql_en.dmg)

安装分析工具和`Navicat for MySQL`

``` bash
$ brew install class-dump
$ brew cask install hopper-disassembler
$ brew cask install navicat-for-mysql
```

## 界面分析

启动`Navicat for MySQL`，弹出试用过期的界面

![Navicat过期](/assets/images/reverse-navicat-for-mysql-on-osx/navicat-expired.png)

试着从提示中提取一些关键字

* 14-day
* expired

## 静态分析

将`Navicat for MySQL`的可执行文件拷贝到桌面

``` bash
$ cp /Applications/Navicat\ for\ MySQL.app/Contents/MacOS/Navicat\ for\ MySQL ~/Desktop/navicat
```

启动`Hopper Disassembler`，打开上述的navicat文件，稍等片刻等文件解析完成。

搜索字符串`14-day`，得到两条信息

![Hopper中搜索14-day](/assets/images/reverse-navicat-for-mysql-on-osx/hopper-search-14-day.png)

第一条"This is a limited 14-day shareware version of PremiumSoft Navicat for evaluation purposes."正是我们要找的关键信息。

选中该字符串，按快捷键`x`（或者选择`Navigate` - `References to Highlighted Word`）打开引用界面，选择合适的项，持续搜索，最终找到`+[Registration ApplicationChecking:isLaunch:]`

![14-day的引用](/assets/images/reverse-navicat-for-mysql-on-osx/hopper-application-checking.png)

似乎看到了希望。

任意选择一条点击进去，查看`+[Registration ApplicationChecking:isLaunch:]`的汇编指令

![ApplicationChecking的汇编指令](/assets/images/reverse-navicat-for-mysql-on-osx/hopper-application-checking-asm-code.png)

汇编还不过关，在工具栏上选择伪代码模式

{% highlight c linenos %}
char +[Registration ApplicationChecking:isLaunch:](void * self, void * _cmd, void * arg2, char arg3) {
    var_8C = arg3;
    r13 = self;
    var_B8 = [arg2 retain];
    rax = [Registration _checkKeyAgainstPlist];
    if (rax == 0xffffffff) goto loc_10028d205;

loc_10028d148:
    var_38 = rax;
    if ((HIBYTE(rax) & 0x30) == 0x0) goto loc_10028d1c6;

loc_10028d150:
    r14 = [[Registration alloc] init];
    rbx = [[r14 getLicenseDataFromPlist] retain];
    r15 = [r14 isLicenseDataVaild:rbx doCheckTime:0x0 createLicenseDict:0x0 createRegInfoDict:0x0];
    [rbx release];
    [r14 release];
    rax = var_38;
    if (r15 == 0x0) goto loc_10028d205;

loc_10028d1c6:
    rbx = rax & 0x1000;
    rcx = rbx == 0x0 ? 0x1 : 0x0;
    if (((HIBYTE(rax) & 0x60) != 0x0) || (rcx == 0x0)) goto loc_10028d320;

loc_10028d1e2:
    r13 = 0x0;
    var_88 = 0x0;
    r15 = 0x0;
    var_38 = 0x0;
    var_30 = 0x0;
    r14 = 0x0;
    goto loc_10028f209;

loc_10028f209:
    [r13 release];
    [var_88 release];
    [r15 release];
    [var_38 release];
    [var_30 release];
    [var_B8 release];
    rax = sign_extend_64(r14);
    return rax;

loc_10028d320:
    if ([r13 respondsToSelector:@selector(CheckNFRExpired:)] != 0x0) {
            rax = var_38 & 0x2000;
            rcx = 0x1e;
            if (rax != 0x0) {
                    rcx = 0x5a;
            }
            rdx = 0x16d;
            if (rbx == 0x0) {
                    rdx = rcx;
            }
            rax = [Registration CheckNFRExpired:rdx];
            *(int32_t *)_remaindays = rax;
    }
    else {
            rax = *(int32_t *)_remaindays;
    }
    if (rax < 0x0) goto loc_10028d69f;

loc_10028d67f:
    r13 = 0x0;
    var_88 = 0x0;
    var_38 = 0x0;
    var_30 = 0x0;
    r14 = 0x1;
    goto loc_10028f1e4;

loc_10028f1e4:
    r15 = 0x0;
    goto loc_10028f209;

loc_10028d69f:
    var_70 = r13;
    ...
{% endhighlight %}

简单分析一下流程：

1. 调用`[Registration _checkKeyAgainstPlist]`检查是否已经注册过，尚未注册(-1)则执行步骤2，否则步骤4
2. 调用`[Registration CheckExpired]`看还有几天试用期
3. 判断是否在试用期
  * 在试用期，则允许试用
	* 否则，提示过期并禁止试用
4. 已经注册过，如果注册结果的`HIBYTE`位包含`0x30`则执行步骤5；否则步骤6
5. 检查`License`是否过期，过期则执行步骤3；否则步骤6
6. 注册结果包含`0x1000`或者`HIBYTE`包含`0x60`，则执行步骤7；否则执行步骤8
7. 调用`[Registartion CheckNFRExpired]`检查还剩时间，有则执行步骤8；否则`License`过期，提示重新购买
8. 做一些清理工作，并继续使用

> 整个流程似乎有个Bug，从步骤6可不检查有效期等限制，直接跳到步骤8，继续使用。

根据上述流程，初步有2个使用方案

1. 不注册，无限期试用（步骤1、2、3）
2. 跳过注册，直接使用（步骤1、4、6、8）


这里提供一下方案2的思路。

## Swizzle Methods

方案2需要Swizzle的方法有

* `[Registration _checkKeyAgainstPlist]`返回`0x60 << 16`
* `[Registartion CheckNFRExpired:]`返回任意正整数（由于上述Bug的原因非必需）

可以用`class-dump`获取头文件, 并从头文件中查找`SEL`原型

``` bash
$ class-dump navicat > header.h
```

## 尾声

对`Navicat for MySQL`的逆向分析就此告一段落，也算是对逆向有个实践性的认识。待汇编功底补上来之后，再尝试从汇编的角度去看一看。

参考文档:

* [对LOWORD, HIWORD, LOBYTE, HIBYTE的理解](http://blog.csdn.net/zhanglidn013/article/details/17283265)

