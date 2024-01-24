---
title: Hexo解决进入页面动画加载缓慢问题
date: 2024-01-23 23:50:22
author: 长白崎
categories:
  - "Hexo"
tags:
  - "Hexo"
---



# Hexo解决进入页面动画加载缓慢问题

---

是否开启了进入页面启动加载动画，并且加载非常缓慢？

导致这个的原因大致有：

1. 动画加载费时
2. 浏览器需要将网站整体的元素都初始化完成，导致耗费时间
3. 浏览器初始化网站时需要返回一个值，但一直没有返回
4. 你的网站它心情不太好🧐



Anyway，想到一个粗暴但简单的方法来解决这个问题。

## 解决办法

打开 `\themes\butterfly\layout\includes\loading\fullpage-loading.pug`文件

将标注`//新增`的两行代码添加进去

```
#loading-box
  .loading-left-bg
  .loading-right-bg
  .spinner-box
    .configure-border-1
      .configure-core
    .configure-border-2
      .configure-core
    .loading-word= _p('loading')

//- script. //原代码
script(async). //新增
  const preloader = {
    endLoading: () => {
      document.body.style.overflow = 'auto';
      document.getElementById('loading-box').classList.add("loaded")
    },
    initLoading: () => {
      document.body.style.overflow = '';
      document.getElementById('loading-box').classList.remove("loaded")

    }
  }
  window.addEventListener('load',()=> { preloader.endLoading() })
  setTimeout(function(){preloader.endLoading();}, 5000); //新增

  if (!{theme.pjax && theme.pjax.enable}) {
    document.addEventListener('pjax:send', () => { preloader.initLoading() })
    document.addEventListener('pjax:complete', () => { preloader.endLoading() })
  }
```

这两行代码的意思是：

在加载动画时，采用的是异步加载方式。如果超过5秒没有反应，则认为超时。



*参考文章：[Loading Animation | Akilarの糖果屋](https://akilar.top/posts/3d221bf2/)*
