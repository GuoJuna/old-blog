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

## 查询树结构

- 创建函数
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

- 使用函数
```
select * from sp_member_relation where FIND_IN_SET(p_open_id, getChildList(1));
```

- 查询所有函数与存储过程
```
 SELECT * FROM information_schema.Routines
```

- 查询单个函数
```
show create function getChildList
```

- 函数调用
```
select getChildList("o_RE65KroiKWbuzBUT05KjTEMfQM")
```