---
layout: post
title: Docker 入门教程 (三)
category: Docker
tags: [java,Docker]
excerpt: Dockerfile 指令详解
---

## FROM 指定基础镜像
FROM 指令用于指定其后构建新镜像所使用的基础镜像。FROM 指令必是 Dockerfile 文件中的首条命令，启动构建流程后，Docker 将会基于该镜像构建新镜像，FROM 后的命令也会基于这个基础镜像。
FROM语法格式为：
```
FROM <image>
```
或
```
FROM <image>:<tag>
```
或
```
FROM <image>:<digest>
```
通过 FROM 指定的镜像，可以是任何有效的基础镜像。FROM 有以下限制：

- FROM 必须 是 Dockerfile 中第一条非注释命令
- 在一个 Dockerfile 文件中创建多个镜像时，FROM 可以多次出现。只需在每个新命令 FROM 之前，记录提交上次的镜像 ID。
- tag 或 digest 是可选的，如果不使用这两个值时，会使用 latest 版本的基础镜像

## RUN 执行命令
在镜像的构建过程中执行特定的命令，并生成一个中间镜像。格式:
```
#shell格式
RUN <command>
#exec格式
RUN ["executable", "param1", "param2"]
```
- RUN 命令将在当前 image 中执行任意合法命令并提交执行结果。命令执行提交后，就会自动执行 Dockerfile 中的下一个指令。
- 层级 RUN 指令和生成提交是符合 Docker 核心理念的做法。它允许像版本控制那样，在任意一个点，对 image 镜像进行定制化构建。
- RUN 指令创建的中间镜像会被缓存，并会在下次构建中使用。如果不想使用这些缓存镜像，可以在构建时指定 --no-cache 参数，如：docker build --no-cache。

## COPY 复制文件
格式：
```
COPY <源路径>... <目标路径>
COPY ["<源路径1>",... "<目标路径>"]
```
和 RUN 指令一样，也有两种格式，一种类似于命令行，一种类似于函数调用。COPY 指令将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的<目标路径>位置
```
COPY package.json /usr/src/app/
```
<源路径>可以是多个，甚至可以是通配符，其通配符规则要满足 Go 的 filepath.Match 规则，如：
```
COPY hom* /mydir/
COPY hom?.txt /mydir/
```

## ADD 更高级的复制文件
ADD 指令和 COPY 的格式和性质基本一致。但是在 COPY 基础上增加了一些功能。比如<源路径>可以是一个 URL，这种情况下，Docker 引擎会试图去下载这个链接的文件放到<目标路径>去
在构建镜像时，复制上下文中的文件到镜像内，格式：
```
ADD <源路径>... <目标路径>
ADD ["<源路径>",... "<目标路径>"]
```

## ENV 设置环境变量
格式有两种：
```
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2>...
```
示例
```
ENV VERSION=1.0 DEBUG=on \
    NAME="Happy Feet"
```
这个例子中演示了如何换行，以及对含有空格的值用双引号括起来的办法，这和 Shell 下的行为是一致的。

## EXPOSE
为构建的镜像设置监听端口，使容器在运行时监听。格式：
```
EXPOSE <port> [<port>...]
```
EXPOSE 指令并不会让容器监听 host 的端口，如果需要，需要在 docker run 时使用 -p、-P 参数来发布容器端口到 host 的某个端口上。
