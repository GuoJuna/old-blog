---
layout: post
title: Mysql 树形结构表设计
category: mysql
tags: [java,mysql]
excerpt: mysql 无限层级设计
---

## 需求
A邀请B  B邀请C C邀请D D邀请E 无限邀请层级
想根据A查出 BCD
根据B 查出CD
根据C 查出D 

## 解决方案

- 邻接表:依赖父节点

- 路径枚举

- 嵌套集

- 闭包表

## 总结

最后选择领接表的方案 
- 表数量少
- 表设计简单

### 创建表
```
CREATE TABLE `sp_member_relation` (
  `p_open_id` varchar(50) NOT NULL COMMENT '邀请人openId',
  `p_tel` varchar(11) DEFAULT NULL COMMENT '邀请人电话',
  `open_id` varchar(50) NOT NULL COMMENT '被邀请人openId',
  `tel` varchar(11) DEFAULT NULL COMMENT '被邀请人tel',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  PRIMARY KEY (`p_open_id`,`open_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='会员关系';
```
### 获取所有子节点

创建mysql 函数获取所有子节点
```
DELIMITER $$
USE `pv_web2`$$

DROP FUNCTION
IF EXISTS `getChildList`$$

CREATE DEFINER = `root`@`%` FUNCTION `getChildList` (rootId VARCHAR(50)) RETURNS VARCHAR (1000) CHARSET utf8
BEGIN
	DECLARE
		sChildList VARCHAR (1000) ; DECLARE
			sChildTemp VARCHAR (1000) ;
		SET sChildTemp = rootId;
		WHILE sChildTemp IS NOT NULL DO

		IF (sChildList IS NOT NULL) THEN

		SET sChildList = CONCAT(sChildList, ',', sChildTemp) ;
		ELSE

		SET sChildList = CONCAT(sChildTemp) ;
		END
		IF ; SELECT
			GROUP_CONCAT(open_id) INTO sChildTemp
		FROM
			sp_member_relation
		WHERE
			FIND_IN_SET(p_open_id, sChildTemp) > 0 ;
		END
		WHILE ; RETURN sChildList ; END$$

DELIMITER ;
```

使用函数
```
select * from sp_member_relation where FIND_IN_SET(p_open_id, getChildList(#{openId,jdbcType=VARCHAR}))
```

### 获取所有父节点

创建函数   
```
DELIMITER $$
USE `pv_web2`$$

DROP FUNCTION
IF EXISTS `getSupList`$$

CREATE DEFINER = `root`@`%` FUNCTION `getSupList` (rootId VARCHAR(50)) RETURNS VARCHAR (1000) CHARSET utf8
BEGIN
	DECLARE
		sChildList VARCHAR (1000) ; DECLARE
			sChildTemp VARCHAR (1000) ;
		SET sChildTemp = rootId;
		WHILE sChildTemp IS NOT NULL DO

		IF (sChildList IS NOT NULL) THEN

		SET sChildList = CONCAT(sChildList, ',', sChildTemp) ;
		ELSE

		SET sChildList = CONCAT(sChildTemp) ;
		END
		IF ; SELECT
			GROUP_CONCAT(p_open_id) INTO sChildTemp
		FROM
			sp_member_relation
		WHERE
			FIND_IN_SET(open_id, sChildTemp) > 0 ;
		END
		WHILE ; RETURN sChildList ; END$$

DELIMITER ;
```

使用函数
```
select * from sp_member_relation where FIND_IN_SET(open_id, getSupList("44"));
```

> https://blog.csdn.net/sinat_33261247/article/details/91492396

