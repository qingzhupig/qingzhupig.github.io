---
layout: post
title: "创建一个cask"
categories: brew, cask
---

## 0x01 源起

有时候想安装的软件在官方的`cask`里搜不到，只能到软件的官网去下载本地安装。
本地安装多少有点麻烦，而且后续的维护也比较麻烦，所以尝试自己做一个`cask`。

以[`腾讯视频`](http://v.qq.com/download.html#mac)为例。

## 0x02 获取信息说明

[下载](https://dldir1.qq.com/qqtv/mac/TencentVideo_V2.3.1.43012.dmg)安装文件，双击打开，会自动挂载到`/Volumn/腾讯视频`。

```bash
$ ls /Volumn/腾讯视频
Applications QQLive.app
$ plutil -p /Volumn/腾讯视频/QQLive.app/Contents/Info.plist | grep CFBundleIdentifier
  "CFBundleIdentifier" => "com.tencent.tenvideo"
$ shasum -a 256 TencentVideo.dmg
503a89359032692d5651c75073c20705e95da6c6b94e86c3e98120d442490f3c TencentVideo.dmg
```

`zap`段参考`qqmusic`的`cask`。


## 0x03 创建cask

``` bash
$ brew cask create tenvideo
# 填写相关信息，最后内容如下
$ brew cask cat tenvideo
cask 'tenvideo' do
  version '2.3.1.43012'
  sha256 '503a89359032692d5651c75073c20705e95da6c6b94e86c3e98120d442490f3c'

  url "https://dldir1.qq.com/qqtv/mac/TencentVideo_V#{version}.dmg"
  name '腾讯视频'
  homepage 'http://v.qq.com'

  app 'QQLive.app'

  uninstall quit: 'com.tencent.tenvideo'

  zap trash: [
              '~/Library/Caches/com.tencent.tenvideo',
              '~/Library/Containers/com.tencent.tenvideo',
              '~/Library/Preferences/com.tencent.tenvideo.plist',
              '~/Library/Saved Application State/com.tencent.tenvideo.savedState',
              ]
end
```

创建好之后就可以通过`brew cask install tenvideo`来安装，通过`brew cask zap tenvideo`来卸载。

更新需要通过`brew cask edit tenvideo`来修改版本号和`sha256`的值。

## 0x04 参考

[Adding a cask](https://github.com/Homebrew/homebrew-cask/blob/master/doc/development/adding_a_cask.md)
