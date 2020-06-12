---
layout: post
title: 使用vuepress创建博客
category: java
tags: [java,github]
excerpt: 使用vuepress创建博客
---



## 快速上手

1. 创建并进入一个新目录

   ```sh
   mkdir blog-vue && cd blog-vue
   ```

2. 使用你喜欢的包管理器进行初始化

   ```sh
   yarn init # npm init
   ```

3. 将 VuePress 安装为本地依赖

   ```sh
   yarn add -D vuepress # npm install -D vuepress
   ```

4. 创建你的第一篇文档

   ```sh
   mkdir docs && echo '# Hello VuePress' > docs/README.md
   ```

5. 在 `package.json` 中添加一些 [scripts](https://classic.yarnpkg.com/zh-Hans/docs/package-json#toc-scripts)

   ```
   {
     "scripts": {
       "docs:dev": "vuepress dev docs",
       "docs:build": "vuepress build docs"
     }
   }
   ```

6. 在本地启动服务器

   ```sh
   yarn docs:dev # npm run docs:dev
   ```

   VuePress 会在 [http://localhost:8080](http://localhost:8080/) 启动一个热重载的开发服务器。