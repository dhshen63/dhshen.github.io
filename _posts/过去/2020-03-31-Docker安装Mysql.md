---
layout: post
title: "Docker安装Mysql"
date: 2020-03-31
tag: 过去
---
### 一、前言

> * 安装Docker后，又安装MySQL。这几天可能确实有点时间🤣
> * 安装了一个MySQL8的版本，遇到了一点小问题，下面详细说明

### 二、拉取镜像

> * **直接拉取官方镜像**
>   - 可以先搜索一下docker镜像
>
>```shell
> docker search mysql #(镜像名)
>```
>   - 拉取MySQL镜像（没有指定版本默认拉取最新版）
>
>```shell
> docker pull mysql 
>```
> * **从指定网站拉取指定的版本**
>   - 只要你会拉取MySQL的镜像，其他镜像应该也会了。
>   - 通过国内[镜像网站](https://www.daocloud.io/)（之前的文章也说过）拉取镜像。
>   - 登录后，点击【发现镜像】。找自己需要的镜像，并选择版本
>
> ![Docker](/images/Docker/001.jpg)
>
>```shell
> docker pull daocloud.io/library/mysql:5.7.4
>```

### 三、启动MySQL容器

> * **注意:**
>   - 启动MySQL容器时，如果镜像没有下载，会自动下载后，再启动。如果已经下载，那么会直接启动。
>   - 下面的命令安装低版本的应该没问题(之前用过没问题，这次没有验证，有问题再说)😂。
>   - 安装最新版的MySQL，可能报错，下面有解决方案
>
>```shell
> #推荐使用docker-compose，就不用每次敲这么长的命令了。以后可能会更新docker-compose的使用教程
> docker run -p 3306:3306 --name mysql \
> -v /usr/local/docker/mysql/conf:/etc/mysql \
> -v /usr/local/docker/mysql/logs:/var/log/mysql \
> -v /usr/local/docker/mysql/data:/var/lib/mysql \
> -e MYSQL_ROOT_PASSWORD=123456 \
> -d mysql
>```
> * **命名参数:**
>   - -p 3306:3306 ：将容器的3306端口映射到主机的3306端口
>   - -v /usr/local/docker/mysql/conf:/etc/mysql ：将主机当前目录下的 conf 挂载到容器的/etc/mysql 
>   - -v /usr/local/docker/mysql/logs:/var/log/mysql ：将主机当前目录下的 logs 目录挂载到容器的 /var/log/mysql 
>   - -v /usr/local/docker/mysql/data:/var/lib/mysql ：将主机当前目录下的 data 目录挂载到容器的 /var/lib/mysql 
>   - -e MYSQL\_ROOT\_PASSWORD=123456 ：初始化root用户的密码
>
> * **docker容器常用命令**
>   - 使用下面命令验证是否有问题。
>
>```shell
> #查看docker容器运行日志
> docker logs (容器ID)
> #查看正在运行的容器
> docker ps
> #查看所有的容器
> docker ps -a
> #查看最近的运行容器
> docker ps -l
> #停止运行的容器
> docker stop (容器ID)  
> #删除容器（必须先停止）
> docker rm (容器ID)
> #进入docker容器
> docker exec -it container-id /bin/bash
>```

### 四、遇到的问题

> * 这里记录下docker安装MySQL8的时候遇到的问题
>   - 通过查看日志，报这个错误：`docker mysql mysqld: Error on realpath() on '/var/lib/mysql-files' No such file or directory`
> * 解决：
>   - 在上面的启动容器的命令中添加下面这个
>
>```shell
> #这个挂载文件经查询，未找到相关文档
> -v /home/mysql/mysql-files:/var/lib/mysql-files
>```

<br>
    
转载请注明：[Memory的博客](https://www.shendonghai.com) » [点击阅读原文](http://www.shendonghai.com/2020/03/Docker%E5%AE%89%E8%A3%85Mysql/) 