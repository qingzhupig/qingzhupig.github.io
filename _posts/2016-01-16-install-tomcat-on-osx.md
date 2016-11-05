---
layout: post
title: "从源码安装Tomcat"
date: 2016-01-16 12:33:10 +0800
categories: 
---

## 工具
> tar, ant

```bash
$ brew install tar ant
```

## 下载
  下载稳定版本[Tomcat 8][1].

## 编译
``` bash
# 将下载下来的文件重命名为tomcat.tar.gz
$ tar xvzf tomcat.tar.gz
$ cd tomcat
$ cp build.properties.default build.properties
# 修改build.properties, 设置bash.path属性，比如./dependencies
$ ant
# 编译开始，待编译完成之后，可以在output/build中找到
$ sudo mkdir -p /usr/local/tomcat/
$ sudo cp -r output/build/ /usr/local/tomcat/
$ sudo chown -R `whoami` /usr/local/tomcat/
```

## 配置
> 默认情况下，tomcat的http端口号是8080，https端口号是8443；
> 可以在tomcat/conf/server.xml中修改

## 部署
> 首先需要有一个打包好的web应用，直接放到tomcat/webapps下即可

## 运行
``` bash
# 通常不需要用root账户来运行tomcat，
# 可使用普通账号或者tomcat专属用户来运行
$ sh bin/startup.sh
# 或者
$ sh bin/catalina.sh start

# 停止运行 (两种方法都可以)
$ sh bin/shutdown.sh
# 或者
$ sh bin/catalina.sh stop

# 如果需要打开调试端口，可以
$ sh bin/catalina jpda start

# 默认使用8000端口作为调试端口，
# 可以修改tomcat/bin/catalina.sh的JPDA_ADDRESS配置使用其他端口
```

[1]: http://apache.opencas.org/tomcat/tomcat-8/v8.0.30/src/apache-tomcat-8.0.30-src.tar.gz


