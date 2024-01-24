---
title: Java学习踩坑笔记
date: 2023-07-19 20:08:59
author: 长白崎
categories:
  - "Java"
tags:
  - "Java"
---



# Java学习踩坑笔记

---

## Java如何在打包成jar包之后也能读取resource文件夹里面的文件？

> ```java
>             //获取文件的URL
>             URL url = new HintUtil().getClass().getClassLoader().getResource("notice.wav");
>             InputStream resourceAsStream = new HintUtil().getClass().getResourceAsStream("/notice.wav");
>             BufferedInputStream myStream = new BufferedInputStream(resourceAsStream);
>             as = AudioSystem.getAudioInputStream(myStream);
> ```
>
> ![读取resource](./Java学习踩坑笔记/images/读取resource.png)
>
> 这里提供了一个获取resource文件流的例子，亲测有效！！！。