---
layout: post
title: 2020 java面试整理
category: java
tags: [java,myBatis]
excerpt: java面试整理
---

## java基础


## 多线程


## jvm


## Spring


## mysql


## Spring boot

### spring boot优势
（1）独立运行的Spring项目
Spring Boot可以以jar包的形式进行独立的运行，使用：java -jar xx.jar 就可以成功的运行项目，或者在应用项目的主程序中运行main函数即可；
（2）内嵌的Servlet容器
内嵌容器，使得我们可以执行运行项目的主程序main函数，并让项目的快速运行；
（3）提供starter简化Maven配置
Spring Boot提供了一系列的starter pom用来简化我们的Maven依赖
（4）自动配置Spring
Spring Boot会根据我们项目中类路径的jar包/类，为jar包的类进行自动配置Bean，这样一来就大大的简化了我们的配置。当然，这只是Spring考虑到的大多数的使用场景，在一些特殊情况，我们还需要自定义自动配置；
（5）应用监控
Spring Boot提供了基于http、ssh、telnet对运行时的项目进行监控；

### 自动配置原理
Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。

## MyBatis


## Spring cloud


## redis


## RabbitMQ

