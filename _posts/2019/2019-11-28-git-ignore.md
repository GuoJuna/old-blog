---
layout: post
title: git配置文件
copyright: java
category: java
tags: [java,git]
excerpt: gitignore 文件
---

## 介绍
有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...，看着肯定不爽。

好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

## 文件规则

```
# 以'#'开始的行，被视为注释.                                                                                                                          

# 忽略掉所有文件名是 foo.txt的文件.

foo.txt

# 忽略所有生成的 html文件,

*.html

# foo.html是手工维护的，所以例外.

!foo.html

# 忽略所有.o和 .a文件.

*.[oa]
配置语法：
以斜杠“/”开头表示目录；
以星号“*”通配多个字符；
以问号“?”通配单个字符
以方括号“[]”包含单个字符的匹配列表；
以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；
```

## 文件模板
```
### gradle ###
.gradle
/build/
!gradle/wrapper/gradle-wrapper.jar

### STS ###
.settings/
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
bin/

### IntelliJ IDEA ###
/.idea/
/private/
/storage/
/litemall.iml
.checkstyle
.idea
*.iws
*.iml
*.ipr
rebel.xml
### maven ###
target/
*.war
*.ear
*.zip
*.tar
*.tar.gz

### logs ####
/logs/
*.log

### temp ignore ###
*.cache
*.diff
*.patch
*.tmp
*.java~
*.properties~
*.xml~

### system ignore ###
.DS_Store
Thumbs.db
Servers
.metadata
upload
gen_code
```

## 其他
```
# 添加被忽略的文件,可以用-f强制添加到Git
git add -f App.class

# 检查gitignore文件
git check-ignore -v App.class
```