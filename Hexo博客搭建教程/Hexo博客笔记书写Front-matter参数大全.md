---
title: Hexo博客笔记书写Front-matter参数大全
date: 2024-01-14 23:58:08
author: 长白崎
categories:
  - "Hexo"
tags:
  - "Hexo"
---



# Hexo博客笔记书写Front-matter参数大全

---

## 常见Front-matter

```txt
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 长白崎
img: /source/images/xxx.jpg
top: true
hide: false
cover: true
published: false
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
---
```

## 参数说明

* title 文章标题
* date 文章发布时间
* author 文章作者
* img 标题图片
* top 是否置顶true为置顶
* hide 是否隐藏文章true为隐藏
* published 是否公开文章false为不公开

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

