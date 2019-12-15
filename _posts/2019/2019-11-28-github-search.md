---
layout: post
title: github 高级搜索
category: git
tags: [java,github]
excerpt: github 高级搜索
---

## 介绍
GitHub 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。
GitHub 于 2008 年 4 月 10 日正式上线，除了 Git 代码仓库托管及基本的 Web 管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目 Ruby on Rails、jQuery 等。

## 搜索


### 一、明确搜索仓库标题、仓库描述、README

1.只想查找仓库名称包含XX的仓库。语法：

　　 in:name 关键词

2.查找描述的内容

　　in:descripton 关键词

3.查README文件包含特定关键词

　　in:readme 关键词

### 二、明确搜索 star、fork 数大于多少的

1. star 数大于 1000 的XX 仓库

　　stars:> 数字 关键字

2.star 数在某个区间 的XX 仓库

　　stars: 10..20 关键词

### 三、明确搜索仓库大小的

搜索限定size的仓库

　　size:>=5000 关键词

注意：这个数字代表K, 5000代表着5M

### 四、明确仓库是否还在更新维护

　　指定时间之前或之后创建或更新的仓库

 　　pushed:>2019-01-03 关键字    //在xx时间后还有更新

　　 created:>2019-01-03 关键字   //在xx时间后创建

### 五、明确搜索仓库的 LICENSE

　　寻找协议为 Apache License 2 的代码

　　license:apache-2.0 关键词

### 六、明确搜索仓库的语言

　　language:java 关键词

### 七、明确搜索某个人或组织的仓库
```
    user:userName

    user:userName language:java

    org:spring-cloud
```   
    