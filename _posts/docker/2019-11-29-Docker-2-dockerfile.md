---
layout: post
title: Docker 入门教程 (二)
category: Docker
tags: [java,Docker]
excerpt: 使用 Dockerfile 定义镜像，依赖镜像来运行容器
---

## Dockerfile 简介
Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

镜像的定制实际上就是定制每一层所添加的配置、文件。如果我们可以把每一层修改、安装、构建、操作的命令都写入一个脚本，用这个脚本来构建、定制镜像，那么之前提及的无法重复的问题、镜像构建透明性的问题、体积的问题就都会解决。这个脚本就是 Dockerfile。

首先通过一张图来了解 Docker 镜像、容器和 Dockerfile 三者之间的关系。
![](assets/images/2019/docker/docker-dockerfile.png)

## Dockerfile 文件格式
Dockerfile文件格式如下：
```
##  Dockerfile文件格式

# This dockerfile uses the ubuntu image
# VERSION 2 - EDITION 1
# Author: docker_user
# Command format: Instruction [arguments / command] ..
 
# 1、第一行必须指定 基础镜像信息
FROM ubuntu
 
# 2、维护者信息
MAINTAINER docker_user docker_user@email.com
 
# 3、镜像操作指令
RUN echo "deb https://archive.ubuntu.com/ubuntu/ raring main universe" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
 
# 4、容器启动执行指令
CMD /usr/sbin/nginx
```
Dockerfile 分为四部分：**基础镜像信息**、**维护者信息**、**镜像操作指令**、**容器启动执行指令**。一开始必须要指明所基于的镜像名称，接下来一般会说明维护者信息；后面则是镜像操作指令，例如 RUN 指令。每执行一条RUN 指令，镜像添加新的一层，并提交；最后是 CMD 指令，来指明运行容器时的操作命令。

## 构建镜像
docker build 命令会根据 Dockerfile 文件及上下文构建新 Docker 镜像。构建上下文是指 Dockerfile 所在的本地路径或一个URL（Git仓库地址）。构建上下文环境会被递归处理，所以构建所指定的路径还包括了子目录，而URL还包括了其中指定的子模块。

将当前目录做为构建上下文时，可以像下面这样使用docker build命令构建镜像：
```
docker build .
Sending build context to Docker daemon  6.51 MB
...
```
Dockerfile 一般位于构建上下文的根目录下，也可以通过-f指定该文件的位置
```
docker build -f /path/to/a/Dockerfile .
```
构建时，还可以通过-t参数指定构建成镜像的仓库、标签。

## 镜像标签
```
docker build -t nginx/v3 .
```
如果存在多个仓库下，或使用多个镜像标签，就可以使用多个-t参数：
```
docker build -t nginx/v3:1.0.2 -t nginx/v3:latest .
```

## 简单示例
```
# 1.创建Dockerfile文件
mkdir mynginx
cd mynginx
vi Dockerfile

# 2.添加Dockerfile文件内容,这个 Dockerfile 很简单，一共就两行涉及到了两条指令：FROM 和 RUN，FROM 表示获取指定基础镜像，RUN 执行命令，在执行的过程中重写了 nginx 的默认页面信息，将信息替换为：Hello, Docker!
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html

# 3.执行Dockerfile文件, 命令最后有一个. 表示当前目录
docker build -t nginx:v1 .

# 4.构建完成后,查看所有镜像，如果存在 REPOSITORY 为 nginx 和 TAG 是 v1 的信息，就表示构建成功
docker images

# 5.启动容器, 用 nginx 镜像启动一个容器，命名为docker_nginx_v1，并且映射了 80 端口，这样我们可以用浏览器去访问这个 nginx 服务器
docker run  --name docker_nginx_v1   -d -p 80:80 nginx:v1

# 6.修改容器内容, 容器启动后，需要对容器内的文件进行进一步的完善，可以使用docker exec -it xx bash命令再次进行修改
docker exec -it docker_nginx_v1   bash
root@3729b97e8226:/# echo '<h1>Hello, Docker neo!</h1>' > /usr/share/nginx/html/index.html
root@3729b97e8226:/# exit
exit

# 7.修改了容器的文件，也就是改动了容器的存储层，可以通过 docker diff 命令看到具体的改动
docker diff docker_nginx_v1 
```