---
layout: post
title: Docker 入门教程 (一)
category: Docker
tags: [java,Docker]
excerpt: Docker hello-world快速开始
---

## Docker 简介
Docker 是世界领先的软件容器平台。开发人员利用 Docker 可以消除协作编码时“在我的机器上可正常工作”的问题。运维人员利用 Docker 可以在隔离容器中并行运行和管理应用，获得更好的计算密度。企业利用 Docker 可以构建敏捷的软件交付管道，以更快的速度、更高的安全性和可靠的信誉为 Linux 和 Windows Server 应用发布新功能。

## Docker安装

```
#查看系统信息 centos
cat /etc/redhat-release
```
### CentOS 7 (使用yum进行安装)
```
# 1.安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 2.添加软件源信息
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 3.更新并安装 Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce

# 4.开启Docker服务
sudo service docker start
```
>https://www.jianshu.com/p/66575c930cf1

## Docker Hello world

```
# 1.拉取Hello world镜像
docker pull library/hello-world

# 2.查看Hello world
docker images

# 3.运行Hello world
docker run hello-world
```

## Docker 常用命令
```
# 1.拉取docker镜像
docker pull image_name

# 2.查看宿主机上的镜像，Docker镜像保存在/var/lib/docker目录下:
docker images

# 3.删除镜像
docker rmi  docker.io/tomcat:7.0.77-jre7   或者  docker rmi b39c68b7af30

# 4.查看当前有哪些容器正在运行
docker ps

# 5.查看所有容器
docker ps -a

# 6.启动、停止、重启容器命令
docker start container_name/container_id
docker stop container_name/container_id
docker restart container_name/container_id

# 7.后台启动一个容器后，如果想进入到这个容器，可以使用attach命令
docker attach container_name/container_id

# 8.删除容器的命令
docker rm container_name/container_id

# 9.查看当前系统Docker信息
docker info

# 10.从Docker hub上下载某个镜像, 执行docker pull centos会将Centos这个仓库下面的所有镜像下载到本地repository。
docker pull centos:latest
```