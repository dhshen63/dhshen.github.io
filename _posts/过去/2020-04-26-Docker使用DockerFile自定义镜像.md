---
layout: post
title: "Docker使用DockerFile自定义镜像"
date: 2020-04-26
tag: 过去
---
### 一、说明

> * 这里使用例子来演示DockerFile的使用
> * Docker中运行SpringBoot项目，是通过Docker自定义镜像完成的。
> * 有两种方式可以自定义镜像，这里只介绍一种，也是推荐用的一种。
> * 通过DockerFile文件(一般都用这个名字🤣)自定义镜像

### 二、自定义,并运行镜像

> * 首先需要一个SpringBoot项目(先在本地测通😂)，然后通过Maven打包(一般都是jar包😊)。
> * 然后上传到Linux系统中(建议和DockerFile文件同一个文件夹中)，并创建DockerFile文件，并添加内容
>
>```shell
> sudo vim DockerFile
>```
>
> **文件内容**
>
>```
> FROM williamyeh/java8 # 必须要有一个Java(jdk)的镜像（对应镜像中Java(jdk)的名字，版本随意）
> COPY springboot-demo.jar /usr/local/springboot-demo.jar # 拷贝到工作目录中
> WORKDIR /usr/local #工作目录
> CMD java -jar springboot-demo.jar # cmd执行这个jar文件
>```
>
> **开始构建镜像**
> 
>```shell
> # 注意:后面还有一个. 
> # 仓库名可有可无
> docker bulid -t 仓库名/镜像名:tag . 
> # 例1：docker build -t test/test:1.0 .
> # 例2：docker build -t test:1.0 .
>```
>
> **启动镜像**
>
>```shell
> # -p 映射宿主机端口
> # docker run -itd -p [宿主机端口]:[容器端口] 镜像ID
> docker run -itd -p 8080:8080 40d33c6bcdd9
> # 查看启动的容器id
> docker ps
> # 查看容器启动日志
> docker logs 容器ID
>```
> 
> **访问**
>
> * 通过浏览器访问[宿主机IP]:[端口]/test，比如:192.168.xx.xxx:8080/test
>

<br>
    
转载请注明：[Memory的博客](https://www.shendonghai.com) » [点击阅读原文](http://www.shendonghai.com/2020/04/Docker%E4%B8%AD%E8%BF%90%E8%A1%8C%E8%87%AA%E5%AE%9A%E4%B9%89%E9%95%9C%E5%83%8F/) 