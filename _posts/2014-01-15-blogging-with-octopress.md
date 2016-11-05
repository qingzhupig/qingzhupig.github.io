---
layout: post
title: "用Octopress写博客"
categories: misc
published: false
---

#### 前言
前几天终于把[`Octopress`][1]跑起来，忍不住开始分享。Ejoy2D群里的一位兄弟让我记录一下如何配置和使用[`Octopress`][1]，于是便有了这篇文章（恩，姑且称之为文章）。

简单来说，[`Octopress`][1]是一个能让你把博客建在[`Github`][2] pages上的博客系统，可以像管理软件开发一样来管理博客，其支持[`Markdown`][3]，有众多插件可用来扩展系统。

让我们开始这段奇幻之旅吧。

#### 准备工作
首先，需要有一个[`Github`][2]的账号，会正常的使用[`Github`][2]，详情参阅[帮助文档][4]。 
其次，花上15分钟来学习一下[`Markdown`][3]的语法，so easy！

当然，会一些[`VIM`][5]和[`Make`][6]相关的知识很有帮助。

#### 安装Octopress 
[`Octopress`][1]是用[`Ruby`][7]写的系统，日常的更新和维护主要使用`rake`命令和`git`命令来完成。

[官方文档](http://octopress.org/docs/setup/)

{% highlight bash %}
git clone git://github.com/imathis/octopress.git octopress
cd octopress
gem install bundler
rbenv rehash
bundle install
{% endhighlight %}

以上3-5行，安装一些必须的工具和依赖包，以后重新折腾的时候就没有必要执行了。

> 默认安装gem需要`root`权限，如果只想让当前用户使用，可以添加`--user-install`选项。也可以通过设置环境变量来实现，如下：

``` bash
$ grep GEM ~/.bash_profile
export GEM_HOME="${HOME}/.gem"
export PATH=${GEM_HOME}/bin:$PATH
```

#### 设置Octopress和github pages

当前目录下的Rakefile文件，定义了日常使用和维护常用的`task`：

``` bash 
$ grep 'task :' Rakefile
task :install, :theme do |t, args|
task :generate do
task :watch do
task :preview do
task :new_post, :title do |t, args|
task :new_page, :filename do |t, args|
task :isolate, :filename do |t, args|
task :integrate do
task :clean do
task :update_style, :theme do |t, args|
task :update_source, :theme do |t, args|
task :deploy do
task :gen_deploy => [:integrate, :generate, :deploy] do
task :copydot, :source, :dest do |t, args|
task :rsync do
multitask :push do
task :set_root_dir, :dir do |t, args|
task :setup_github_pages, :repo do |t, args|
task :list do
```

常用的有：
{% highlight bash %}
install		安装主题
generate	生成博客
preview		本地预览
new_post	创建新文章
new_page	创建新页面
deploy		发布博客
{% endhighlight %}

首先给博客设置一下默认的主题，如下：
``` bash
rake install
```

然后在[`Github`][2]上创建一个名为`username.github.io`的仓库，并设置系统。（username是你在[`Github`][2]上的用户名）

``` bash
rake setup_github_pages
```

接下来是博客基础信息：

``` bash
$ head _config.xml
# ----------------------- #
#      Main Configs       #
# ----------------------- #

url: http://xmucoder.com
title: 2018的备忘录
subtitle: 咱也进步一把，欧耶
author: 2018
simple_search: https://www.google.com.hk/search
```

#### 自定义域名
博客默认的URL是`username.github.io`，你也可以使用自己的域名。

{% highlight bash %}
# 设置博客的域名
echo 'your-domain.com' >> source/CNAME
# OR
echo 'www.your-domain.com' >> source/CNAME
{% endhighlight %}

- 对像`www.example.com`的域名，你需要设置一个`CNAME`记录指向`charlie.github.io`
- 对像`example.com`的域名，你需要设置`A`记录指向`192.30.252.153`和`192.30.252.154`


#### 发布博客和提交系统配置
> [`Octopress`][1]通过在[`Github`][2]上使用不同的分支，来分别管理博客内容和系统内容，博客内容使用默认的`master`分支，系统则使用`source`分支。

发布博客使用的是`rake`命令：
``` bash 生成和发布博客
rake generate
rake deploy
```
`rake deploy`把网站`push`到`master`分支上，而我们对本地系统做的修改、配置等，需要直接使用`git`命令来`push`到`source`分支：

``` bash 提交配置
git add .
git commit -m 'init site'
git push origin source
```

至此，一个新的博客就成功发布了，你可以通过前面提到的地址访问你的博客了。

#### 发表文章
接下来是重点了，没有文章的博客当然毫无意义，然而发表文章却是一件很简单的事情，这里不详细说了，请参考如下命令，并查看原始文档。

``` bash
rake new_post["title"]
# edit
rake generate
# (optional)preview
rake preview
rake deploy
```

#### 定制系统

* [我的Octopress配置][10]
* [定制Octopress][11]
* [Generating a Table of Contents in Octopress][12]
* [Remove Redundant /blog Prefix From Octopress][13]
* [Category Pages With Preview and Pagination][14]
* [My Printer Friendly CSS][15]
* [Clone Your Octopress to Blog From Two Places][16]

[1]: http://octopress.org/
[2]: https://github.com/
[3]: http://daringfireball.net/projects/markdown/
[4]: https://help.github.com/
[5]: http://www.vim.org/
[6]: http://www.gnu.org/software/make/
[7]: https://www.ruby-lang.org/en/ 

[10]: http://www.yanjiuyanjiu.com/blog/20130402/
[11]: http://biaobiaoqi.me/blog/2013/07/10/decorate-octopress/
[12]: http://brizzled.clapper.org/blog/2012/02/04/generating-a-table-of-contents-in-octopress/
[13]: http://xit0.org/2013/04/remove-redundant-slash-blog-prefix-from-octopress-website/
[14]: http://highaltitudehacks.com/2013/06/30/octopress-category-pages-with-preview-and-pagination/
[15]: http://qiang.hu/2013/06/my-printer-friendly-css.html
[16]: http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/
