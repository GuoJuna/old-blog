---
layout: post
title: Mysql 使用总结
category: mysql
tags: [java,mysql]
excerpt: mysql 使用总结
---

## mysql 启动/停止/重启MySQL

```
使用 service 启动：service mysqld start

使用 service 启动：service mysqld stop

使用 service 启动：service mysqld restart

```

## 连接
```
mysql -p localhost -u root -p
```

## 授权
grant 权限 on 数据库对象 to 用户
用户后面可以加@'ip地址' identified by '密码'
```
grant all on *.* to gj@'%'IDENTIFIED by '密码'
```