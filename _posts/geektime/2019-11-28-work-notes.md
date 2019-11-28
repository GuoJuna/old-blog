---
layout: post
title: 工作笔记
no-post-nav: true
category: geektime
tags: [geektime]
excerpt: 工作笔记
---

## 1. 查询数据库字符集编码


```
    #查询版本
    select version();
    查询字符集编码
    show variables like '%character%';

    修改客户端字符集
    set NAMES utf8mb4;
    修改character_set_database
    ALTER DATABASE <数据库名> CHARACTER SET = <字符集> COLLATE = <字符集排序规则>;

    说明
    以上参数必须保证除了character_set_filesystem外的参数都统一，才不会出现乱码的情况。
    character_set_client、character_set_connection以及character_set_results是客户端的设置。
    character_set_system、character_set_server以及character_set_database是服务器端的设置。
    服务器端的参数优先级是：

    character_set_database>character_set_server>character_set_system

    https://help.aliyun.com/knowledge_detail/41743.html?spm=5176.13394938.0.0.5666420fBBacn9

```

## 2.查询数据库排序规则

```
    SHOW VARIABLES LIKE 'collation%';
```

## 3.redisTemplate

```
    RedisTemplate中定义了对5种数据结构操作
    //操作字符串
    redisTemplate.opsForValue();
    //操作hash
    redisTemplate.opsForHash();
    //操作list
    redisTemplate.opsForList();
    //操作set
    redisTemplate.opsForSet();
    //操作有序set
    redisTemplate.opsForZSet();
```

## 4.登陆redis

```
./redis-cli -h 127.0.0.1 -p 6379 -a "密码"
```

## 5.mysql 重启
```
    #第一种方式 
    service mysqld restart 
    #第二种方式 
    /etc/init.d/mysql stop 
    /etc/init.d/mysql start
    /etc/init.d/mysql restart
```
## 6.IDEA中Javadoc生成
```
    1.Tools->Generate JavaDoc
     
    2.处理中文编码 -encoding utf-8 -charset utf-8 
```
![](assets/images/2019/geektime/IDEAjavadoc.png)
