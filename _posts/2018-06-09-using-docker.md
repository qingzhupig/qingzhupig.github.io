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

## `Gitlab`

``` bash
$ docker pull gitlab/gitlab-ce
$ sudo mkdir /opt/gitlab
$ sudo chown -R xxx:staff /opt/gitlab
$ mkdir config datas logs
$ docker run -it -d -p 80:80 -p 443:443 -p 22:22 --name gitlab -v /opt/gitlab/config:/etc/config -v /opt/gitlab/logs:/var/log/gitlab -v /opt/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce
## 访问 http://localhost
```

TO BE CONTINUE...
