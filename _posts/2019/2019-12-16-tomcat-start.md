---
layout: post
title: Tomcat 异常:无法启动
category: java
tags: [java,github]
excerpt: tomcat 启动很慢,一直卡在Starting Servlet Engine
---

## 问题
tomcat 启动很慢,一直卡在Starting Servlet Engine

## 
这个问题的起因是tomcat启动的时候，需要通过Linux生成随机数的方式/dev/random生成随机数，但是如果生成随机数失败的话，就会一直阻塞中，这也就是tomcat启动为什么一直卡死某一行日志的原因，经过测试，可以通过配置/dev/urandom来取代/dev/random来解决，我们可以在两个地方进行配置，都可以解决这个问题，取其一即可

### 在jdk安装目录配置
编辑$JAVA_HOME/jre/lib/security/Java.security文件，将securerandom.source=file:/dev/random 换成 securerandom.source=file:/dev/urandom 即可

### 在tomcat配置文件catalina.sh中修改
tomcat的catalina.sh文件位于tomcat目录下的bin目录下，通过在catalina.sh文件添加 -Djava.security.egd=file:/dev/urandom

## 最后 
以上两种解决方式，只需要配置一种即可，在jdk目录中进行配置，可以一劳永逸，不需要每个tomcat中进行配置，但是一般情况下我们对生产环境下的jdk没有操作权限，这时候可以使用在tomcat中配置来进行解决
    配置完之后，再次启动tomcat，可以发现tomcat很快就启动成功了
    
> https://www.jianshu.com/p/2bbcf7a613a5