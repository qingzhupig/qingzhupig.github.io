---
layout: post
title: PD中解决“该应用无法在虚拟机中运行”
date: 2016-04-01 21:33:10 +0800
categories: 
---

# 花絮

偶得知[暴雪](http://www.blizzard.com)在2016年03月15日发布了魔兽争霸III的1.27a补丁，也支持OS X10.3之后的系统；虽然我的系统是OSX 10.9的，但是还是很高兴，第一时间开始下载。

原来暴雪和网易合作，搞了一个[对战平台](http://dz.blizzard.cn)，还举办了一些比赛。正好最近11平台老是出问题，所以打算用用暴雪的对战平台。

下面附上Mac版本游戏下载地址，然后搞起。

#### 下载器

* Reign of Chaos 

  [繁体中文](http://www.battle.net/download/getLegacy?product=WAR3&locale=zh_TW&os=MAC)
  |
  [英文](http://www.battle.net/download/getLegacy?product=WAR3&locale=en_US&os=MAC)

* The Frozen Throne

  [简体中文](http://www.battle.net/download/getLegacy?product=W3XP&locale=zh_TW&os=MAC)
  |
  [英文](http://www.battle.net/download/getLegacy?product=W3XP&locale=en_US&os=MAC)

#### CD-KEY

* Reign of Chaos

  `XRSQWN-U82D-76J7KP-390C-45LPVC`
* The Frozen Throne

  `T4CH2G-E6BF-Y7WZ7Z-6BPK-PRM2MC`

# 运行环境

* OS X 10.9
* Parallel Desktop 9
* Windows XP SP2

# 暴雪对战平台

从官方下载并安装好平台后，第一次执行发现启动不了，提示找不到Kernel32.dll，小小郁闷一下。

想起之前好像看到过，1.27a如果在XP下跑，需要SP3，于是将系统升级到SP3，果然可以打开平台。

平台里有“观战”的功能，试着去看一场DOTA比赛，提示"Sorry, this application cannot run under a Virtual Machine"。

# 应用程序检测虚拟机环境

Google了一下，网上有一些VMWare的解决方案；PD的方案，就是换用VMWare:-)

汗，作为一个曾经以到暴雪做游戏开发为终极目标的前屌丝iOS游戏客户端码农，还是应该象征性挣扎一下的；果然，有work around

# 解决方法

*step by step*

1. Open the start menu in Windows.
2. Search Regedit
3. When regedit is open, open computer, then open HKEY_LOCAL_MACHINE, then open
   HARDWARE, then open DESCRIPTION, then lastly open System.
4. When System is open, find the file that goes by the name of VideoBiosVersion.
5. Delete the content inside VideoBiosVersion.
6. Close regedit, re-open the game that didn't work before (You may have to
   re-do these steps every time you restart your computer),

# 后记

现在很多游戏都会有禁止在虚拟机里跑的功能，大概是为了防止逆向吧，只是这强度似乎...

