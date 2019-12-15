---
layout: post
title: Docker 入门教程 (四)
category: Docker
tags: [java,Docker]
excerpt: Docker 常用命令总结
---

## Docker 环境安装
- 安装yum-utils：
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

- 为yum源添加docker仓库位置
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

- 安装docker
```
yum install docker-ce
```

- 启动docker
```
systemctl start docker
```

## Docker 镜像常用命令

### 搜索镜像
```
docker search java
```

### 下载镜像
```
docker pull java:8
```

### 如何查找镜像支持的版本
```
由于docker search命令只能查找出是否有该镜像，不能找到该镜像支持的版本，所以我们需要通过docker hub来搜索支持的版本。
进入docker hub的官网，地址：https://hub.docker.com
```

### 列出镜像
```
docker images
```

### 删除镜像

- 指定名称删除镜像
```
docker rmi java:8
```

- 指定名称删除镜像（强制）
```
docker rmi -f java:8
```

- 强制删除所有镜像
```
docker rmi -f $(docker images)
```

## Docker 容器常用命令

### 新建并启动容器
```
docker run -p 80:80 --name nginx -d nginx:1.17.0
```
- -d选项：表示后台运行
- --name选项：指定运行后容器的名字为nginx,之后可以通过名字来操作容器
- -p选项：指定端口映射，格式为：hostPort:containerPort

### 列出容器

- 列出运行中的容器
```
docker ps
```

- 列出所有容器
```
docker ps -a
```

### 停止容器
```bash
# $ContainerName及$ContainerId可以用docker ps命令查询出来
docker stop $ContainerName(或者$ContainerId)

docker stop nginx
#或者
docker stop c5f5d5125587
```

### 强制停止容器
```bash
docker kill $ContainerName(或者$ContainerId)
```

### 启动已停止的容器
```
docker start $ContainerName(或者$ContainerId)
```

### 进入容器

- 先查询出容器的pid
```
docker inspect --format "{{.State.Pid}}" $ContainerName(或者$ContainerId)
```

- 根据容器的pid进入容器
```
nsenter --target "$pid" --mount --uts --ipc --net --pid
```

### 删除容器

- 删除指定容器
```
docker rm $ContainerName(或者$ContainerId)
```

- 强制删除所有容器
```
docker rm -f $(docker ps -a -q)
```

### 查看容器的日志
- 查看当前全部日志
```
docker logs $ContainerName(或者$ContainerId)
```

- 动态查看日志
```
docker logs $ContainerName(或者$ContainerId) -f
```

### 查看容器的IP地址
```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $ContainerName(或者$ContainerId)
```

### 同步宿主机时间到容器
```
docker cp /etc/localtime $ContainerName(或者$ContainerId):/etc/
```

### 在宿主机查看docker使用cpu、内存、网络、io情况

- 查看指定容器情况
```
docker stats $ContainerName(或者$ContainerId)
```

- 查看所有容器情况
``` 
docker stats -a
```

### 进入Docker容器内部的bash
``` 
docker exec -it $ContainerName /bin/bash
```

### 修改Docker镜像的存放位置

- 查看Docker镜像的存放位置
```
docker info | grep "Docker Root Dir"
```

- 关闭Docker服务
```
systemctl stop docker
```

- 移动目录到目标路径
``` 
mv /var/lib/docker /mydata/docker
```

- 建立软连接
``` 
ln -s /mydata/docker /var/lib/docker
```