---
layout: post
title: svn 添加账号
category: java
tags: [java,github]
excerpt: Linux 中 svn 添加账号
---

## 操作方法
> 1. 使用Linux命令 find / -name authz
> 2. 进入该目录的conf，其中包含authz、passwd、svnserve.conf三个文件
> 3. 进入passwd，在[users]下面加上你要添加的svn账号及密码
> 4. 再进入authz，在[groups]下加上刚刚添加的用户名
> 5. 重启svn 先kill掉svn进程：killall svnserve   启动svn：sudo svnserve -d -r /opt/svn/repositories/