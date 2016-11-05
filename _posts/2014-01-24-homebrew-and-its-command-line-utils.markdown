---
layout: post
title: "使用Homebrew"
categories: commandline
---

#### 前言
使用`debian`多年，乍换到`mac`下多少有点不习惯。 没有`apt-get`，许多以前常用的命令行小工具也就慢慢不用了，因为安装起来麻烦，也不知道怎么卸载。

幸好现在有了[`homebrew`][1]，一切又简单起来了。

本文记录[`homebrew`][1]和一些工作中用到的命令行工具的安装和使用，便于日后查看。

#### `brew`
> brew - The missing package manager for OS X

{% highlight bash %}
# 安装和使用Homebrew http://brew.sh Homepage
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
$ brew
Example usage:
  brew [info | home | options ] [FORMULA...]
  brew install FORMULA...
  brew uninstall FORMULA...
  brew search [foo]
  brew list [FORMULA...]
  brew update
  brew upgrade [FORMULA...]
  brew pin/unpin [FORMULA...]

Troubleshooting:
  brew doctor
  brew install -vd FORMULA
  brew [--env | --config]

Brewing:
  brew create [URL [--no-fetch]]
  brew edit [FORMULA...]
  open https://github.com/Homebrew/homebrew/wiki/Formula-Cookbook

Further help:
  man brew
  brew home
{% endhighlight %}

> [使用brew cask来安装Mac应用][2]
>
> homebrew-cask - A friendly binary installer for OS X

> [有趣的homebrew命名逻辑][3]
>
> 首先， brew 本身是釀造、釀酒的意思，會用這個字的原因是 homebrew 的安裝方式為下載 source code 回來做編譯，由於是在自己電腦做 local complie 編譯套件，所以這個工具叫做 homebrew 自家釀酒。
> 
> 釀酒需要有配方 formula ，當你需要安裝套件時，流程就是下 brew 命令去根據配方 formula ，釀造出一桶（keg）酒來。所以 keg 指的是整個編譯完成的套件資料夾。
> 
> 再來，放置套件的位置在 /usr/local/Cellar ， Cellar 就是地窖，一桶一桶釀好的酒當然要存放在地窖裡囉！所以編譯完成的套件資料夾 keg 預設目錄在 /usr/local/Cellar 。
> 
> 最後回到「keg-only」這個詞，字面上意思現在就很清楚，表示這個套件只會存放在桶子裡，不會跑出桶子外；實際上的行為是 brew 不會幫你做 symlink 到 /usr/local ，避免你的原生系統內還有一套 readline 而打架，所以訊息提示說 readline 套件是 keg-only 。
> 
> 至此謎題全部解開啦！ Homebrew 的命名邏輯真是超有趣的

#### `Xcode`

`Xcode`是苹果的IDE，其中集成的`Command Line Developer Tools`中包含一些常用的命令行工具，推荐安装。


#### `SoX`

> SoX - Sound eXchange, the Swiss Army knife of audio manipulation

安装SoX

``` bash 
$ brew install sox lame
```
`lame`是`mp3`解码器，当`SoX`处理`mp3`时需要该支持。

`SoX`工具包里有4个工具，分别是`soxi`、`sox`、`play`和`rec`，其中`soxi`用来获取音频信息，`sox`用来处理音频，`play`用来播放，`rec`则用来录音。

`SoX`提供了相对比较复杂的一些功能，可以参看手册页。

比较常见的用法如下：

``` bash 
# 转换音频格式（将input.wav转化为output.mp3，其它格式同样处理）
$ sox input.wav output.mp3
# 截取音频的一部分（截取0到30秒）
$ sox input.mp3 output.mp3 trim 0 30
# 转化为单声道
$ sox input.mp3 output.mp3 remix 1
```
`SoX`在处理音频的时候，自动根据音频文件的信息、命令行参数以及输入、输出文件的扩展名来判定文件的类型。


#### `openssl`

> openssl - OpenSSL command line tool

日常开发中，可以用辅助使用`openssl`来进行编码、加密方面的一些测试：

``` bash
# base64编码和解码
$ echo helloworld | openssl base64
aGVsbG93b3JsZAo=
$ echo aGVsbG93b3JsZAo= | openssl base64 -d
helloworld
# 指定输入或者输出文件
$ openssl base64 [-d] -in in.txt -out out.txt

# MD5和SHA1
$ echo helloworld | openssl md5
d73b04b0e696b0945283defa3eee4538
$ echo helloworld | openssl sha1
e7509a8c032f3bc2a8df1df476f8ef03436185fa

# 加密和解密
$ openssl des3 [-d] -in in.txt -out out.txt
```

[1]: http://brew.sh
[2]: http://blog.devtang.com/blog/2014/02/26/the-introduction-of-homebrew-and-brewcask
[3]: http://wildjcrt.pixnet.net/blog/post/29182044-the-naming-logic-from-homebrew
