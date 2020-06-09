---
layout: post
title: IDEA 配置与问题
category: idea
tags: [java,idea]
excerpt: IDEA 习惯配置与问题
---

## 配置
1. 自动滚动到打开的文件
![](https://gitee.com/guojun49/images/raw/master/blog-img/20200609171549.png)

2. 打开文件乱码
![](https://gitee.com/guojun49/images/raw/master/blog-img/20200609171555.png)

3. 端口占用
 - 启动cmd,执行命令```netstat -ano|findstr  8080```,会查询出占用端口号的进程号
 - ```taskkill -f -pid``` 进程号   杀死进程，然后重启Tomcat即可解决
![](https://gitee.com/guojun49/images/raw/master/blog-img/20200609171600.png)