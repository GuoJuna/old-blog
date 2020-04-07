---
layout: post
title: mybatis-sql执行时间插件
category: java
tags: [java,myBaits]
excerpt: 基于mybatis的一个查看sql执行时间的插件
---


## 代码


```java

package com.bxd.app.util;
import org.apache.ibatis.executor.statement.StatementHandler;
import org.apache.ibatis.mapping.BoundSql;
import org.apache.ibatis.mapping.ParameterMapping;
import org.apache.ibatis.plugin.Interceptor;
import org.apache.ibatis.plugin.Intercepts;
import org.apache.ibatis.plugin.Invocation;
import org.apache.ibatis.plugin.Plugin;
import org.apache.ibatis.plugin.Signature;
import org.apache.ibatis.session.ResultHandler;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

import java.sql.Statement;
import java.util.List;
import java.util.Properties;


/**
 * Sql执行时间记录拦截器
 *
 */
@Intercepts({@Signature(type = StatementHandler.class, method = "query", args = {Statement.class, ResultHandler.class}),
		@Signature(type = StatementHandler.class, method = "update", args = {Statement.class}),
		@Signature(type = StatementHandler.class, method = "batch", args = {Statement.class})})
@Component
public class SqlExecuteTimeCountInterceptor implements Interceptor {

	private static Logger logger = LoggerFactory.getLogger(SqlExecuteTimeCountInterceptor.class);

	/**
	 * 打印的参数字符串的最大长度
	 */
	private final static int MAX_PARAM_LENGTH = 50;

	/**
	 * 记录的最大SQL长度
	 */
	private final static int MAX_SQL_LENGTH = 200;


	@Override
	public Object intercept(Invocation invocation) throws Throwable {
		Object target = invocation.getTarget();
		long startTime = System.currentTimeMillis();
		StatementHandler statementHandler = (StatementHandler) target;
		try {
			return invocation.proceed();
		} finally {
			long endTime = System.currentTimeMillis();
			long timeCount = endTime - startTime;

			BoundSql boundSql = statementHandler.getBoundSql();
			String sql = boundSql.getSql();
			Object parameterObject = boundSql.getParameterObject();
			List<ParameterMapping> parameterMappingList = boundSql.getParameterMappings();

			// 格式化Sql语句，去除换行符，替换参数
			sql = formatSQL(sql, parameterObject, parameterMappingList);

			logger.info("执行 SQL：[ , {} ]执行耗时[ {} ms]", sql, timeCount);
		}
	}


	/**
	 * 格式化/美化 SQL语句
	 *
	 * @param sql                  sql 语句
	 * @param parameterObject      参数的Map
	 * @param parameterMappingList 参数的List
	 * @return 格式化之后的SQL
	 */
	private String formatSQL(String sql, Object parameterObject, List<ParameterMapping> parameterMappingList) {
		// 输入sql字符串空判断
		if (sql == null || sql.length() == 0) {
			return "";
		}
		// 美化sql
		sql = beautifySql(sql);
		// 不传参数的场景，直接把sql美化一下返回出去
		if (parameterObject == null || parameterMappingList == null || parameterMappingList.size() == 0) {
			return sql;
		}
		return LimitSQLLength(sql);
	}


	/**
	 * 返回限制长度之后的SQL语句
	 *
	 *
	 * @param sql 原始SQL语句
	 */
	private String LimitSQLLength(String sql) {
		if (sql == null || sql.length() == 0) {
			return "";
		}
		if (sql.length() > MAX_SQL_LENGTH) {
			return sql.substring(0, MAX_SQL_LENGTH);
		} else {
			return sql;
		}
	}


	@Override
	public Object plugin(Object target) {
		return Plugin.wrap(target, this);
	}

	@Override
	public void setProperties(Properties properties) {

	}




	/**
	 * 替换SQL 中? 所对应的值, 只保留前50个字符
	 *
	 * @param sql     sql语句
	 * @param valueOf ?对应的值
	 */
	private String replaceValue(String sql, String valueOf) {
		//超过50个字符只取前50个
		if (valueOf != null && valueOf.length() > MAX_PARAM_LENGTH) {
			valueOf = valueOf.substring(0, MAX_PARAM_LENGTH);
		}
		sql = sql.replaceFirst("\\?", valueOf);
		return sql;
	}

	/**
	 * 美化sql
	 *
	 * @param sql sql语句
	 */
	private String beautifySql(String sql) {
		sql = sql.replaceAll("[\\s\n ]+", "  ");
		return sql;
	}


}


```

## 使用

mybatis配置文件中添加
```xml

<plugins>
	<plugin interceptor="com.bxd.app.util.SqlExecuteTimeCountInterceptor" />
</plugins>
```