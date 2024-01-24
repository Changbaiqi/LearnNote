---
title: Hexo通过插件对图片进行懒加载
date: 2024-01-23 23:43:36
author: 长白崎
categories:
  - "Hexo"
tags:
  - "Hexo"
---

# Hexo通过插件对图片进行懒加载

---



## 一、下载Hexo-lazyload-image插件

```shell
npm install hexo-lazyload-image --save
```





## 二、修改配置

找到`hexo`项目的根目录下的站点配置文件`_config.yml`，具体路径：`blog/_config.yml`，其中`blog`是你的项目文件夹。

```yml
lazyload:
  enable: true
  onlypost: false # optional
  loadingImg: # optional eg ./images/loading.gif
  isSPA: false # optional
  preloadRatio: 3 # optional, default is 1

```



* onlypost

  * true：只有路由页面或者文章的图片才会被懒加载。

  * false：除了站点背景图（如果有的话），整个站点的图片均会被懒加载。

* loadingImg：指定的话，加载自定义路径的图片用作文章图片加载时显示，不指定的话显示默认图片。
  
* isSPA
    * true：针对单页面应用，当滚动条滚动到图片位置时就会向后端请求图片
    * false：刷新才能请求图片
* preloadRatio：在多少倍的可见区域时触发图片请求，默认为1，即当前屏幕的区域。