---
layout: post
title: Tomcat 重启脚本
category: tomcat
tags: [java,idea]
excerpt: idea重启脚本
---

## tomcat重启脚本
```
#! /bin/bash
#这里配置tomcat的根路径
tomcat_home=/usr/local/tomcat-9090
SHUTDOWN=$tomcat_home/bin/shutdown.sh
echo "Close $tomcat_home"
#$SHUTDOWN
#杀掉tomcat进程
ps -ef |grep tomcat |grep $tomcat_home |grep -v 'grep'|awk '{print $2}' | xargs kill -9
#删除日志文件，如果你不想删除可以不要下面一行
#rm  $tomcat_home/logs/* -rf
#删除tomcat的临时目录
#rm  $tomcat_home/work/* -rf
#暂停5s
sleep 5
echo "Start $tomcat_home"
#跳转到tomcat/bin路径
cd $tomcat_home/bin/
#执行启动tomcat命令
./startup.sh
#查看tomcat日志
#tail -f $tomcat_home/logs/catalina.out
```