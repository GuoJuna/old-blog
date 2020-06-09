---
layout: post
title: Mybatis Plus
category: java
tags: [java]
excerpt: Mybatis Plus
---

## Mybatis plus 
MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高
效率而生。

## 特性
- 无侵入：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- 损耗小：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- 强大的 CRUD 操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- 支持多种数据库：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer2005、SQLServer 等多种数据库
- 支持主键自动生成：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- 支持 XML 热加载：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
- 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ）
- 支持关键词自动转义：支持数据库关键词（order、key......）自动转义，还可自定义关键词
- 内置代码生成器：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，
- 支持模板引擎，更有超多自定义配置等您来使用
- 内置分页插件：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List查询
- 内置性能分析插件：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- 内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
- 内置 Sql 注入剥离器：支持 Sql 注入剥离，有效预防 Sql 注入攻击

## 架构
![](https://gitee.com/guojun49/images/raw/master/blog-img/20200609172822.png)

## 开始
- @TableName 指定数据库表名

- @TableId(type = IdType.AUTO) 指定id类型为自增长

- @TableField注解可以指定字段的一些属性 
    - 对象中的属性名和字段名不一致的问题（非驼峰）   
    - 对象中的属性字段在表中不存在的问题
    
- mybatis-plus.config-location = classpath:mybatis-config.xml
> MyBatis 配置文件位置，如果您有单独的 MyBatis 配置，请将其路径配置到 configLocation 中,

- mybatis-plus.mapper-locations = classpath*:mybatis/*.xml
> MyBatis Mapper 所对应的 XML 文件位置，如果您在 Mapper 中有自定义方法（XML 中有自定义实现），需要进行
  该配置，告诉 Mapper 所对应的 XML 文件位置 

- mybatis-plus.type-aliases-package = cn.itcast.mp.pojo
>MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名，注册后在 Mapper 对应的 XML 文件中可以直接使
  用类名，而不用使用全限定的类名（即 XML 中调用的时候不用包含包名）。

- mybatis-plus.configuration.map-underscore-to-camel-case=false
> 关闭自动驼峰映射，该参数不能和mybatis-plus.config-location同时存在

- mybatis-plus.configuration.cache-enabled=false
> 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存，默认为 true。

- mybatis-plus.global-config.db-config.id-type=auto
> 全局默认主键类型，设置后，即可省略实体对象中的@TableId(type = IdType.AUTO)配置。

- mybatis-plus.global-config.db-config.table-prefix=tb_
> 表名前缀，全局配置后可省略@TableName()配置


## 条件构造器
- 多eq
```
QueryWrapper<Student> queryWrapper = new QueryWrapper<>();
queryWrapper.lambda()
        .eq(Student::getName, "冯文议")
        .eq(Student::getAge, 26);
List<Student> studentList = list(queryWrapper);
for (Student student : studentList)
    Console.info(new Gson().toJson(student));
```

- OR

```
QueryWrapper<Student> queryWrapper = new QueryWrapper<>();
queryWrapper.lambda()
        .eq(Student::getName, "冯文议")
        .or()
        .eq(Student::getName, "1");
List<Student> studentList = list(queryWrapper);
for (Student student : studentList)
    Console.info(new Gson().toJson(student));
```
sql
```
SELECT * FROM t_student WHERE name = ? OR name = ? 
```
