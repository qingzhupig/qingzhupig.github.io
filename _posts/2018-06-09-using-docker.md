---
layout: post
title: "使用Docker"
categories: docker, mysql
---

记录一下[Docker](https://www.docker.com/)常用镜像的使用。

## `MySQL`

``` bash
## 下载镜像
$ docker pull mysql/mysql-server
## 创建容器，设置初始化密码
$ docker run -it -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=xxx --name mysql mysql/mysql-server:latest
## 登陆并创建host可访问的用户
$ docker exec -it mysql mysql -uroot -p 
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'xxxxxx';
mysql> FLUSH PRIVILEGES;
## host中登陆，需要使用127.0.0.1
$ mysql -uroot -p -h 127.0.0.1
```

TO BE CONTINUE...
