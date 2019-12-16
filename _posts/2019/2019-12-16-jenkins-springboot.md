---
layout: post
title: Jenkins 入门教程
category: java
tags: [java,github]
excerpt: 使用Jenkins一键打包部署SpringBoot应用
---

## Jenkins 简介
Jenkins是开源CI&CD软件领导者，提供超过1000个插件来支持构建、部署、自动化，满足任何项目的需要。我们可以用Jenkins来构建和部署我们的项目，比如说从我们的代码仓库获取代码，然后将我们的代码打包成可执行的文件，之后通过远程的ssh工具执行脚本来运行我们的项目。

## Jenkins 的安装与配置

### Docker环境下安装

- 下载Jenkins的Docker镜像
```
docker pull jenkins/jenkins:lts
```

- 在Docker容器中运行Jenkins
```
docker run -p 8080:8080 -p 50000:5000 --name jenkins \
-u root \
-v /mydata/jenkins_home:/var/jenkins_home \
-d jenkins/jenkins:lts
```

- 查看Jenkins日志
```
docker logs -f jenkins
```
