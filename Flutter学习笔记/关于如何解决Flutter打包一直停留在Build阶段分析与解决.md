---
title: 关于如何解决Flutter打包一直停留在Build阶段分析与解决
date: 2025-01-18 02:16:54
author: 长白崎
categories:
  - "Flutter"
tags:
  - "问题解决"
  - "Flutter"
---



# 关于如何解决Flutter打包一直停留在Build阶段分析与解决

---

* 情况：

  若一直出现Build ..一直没有任何打包成功的动静很可能是android对应的Gradle依赖未下载好缘故，一直在下载。而在国内有些依赖是使用的是google的下载渠道，所以网速可能很慢也可能根本就连不上（原因你们也懂的）

* 解决方法：

  首先先进入Flutter先对应的android目录，之后再输入

  ```shell
  ./gradlew clean
  ```

  这样他就会下载对应依赖的同时显示下载进度等，这个时候你就可以分析是下载速度慢的问题还是其他问题导致，如果是下载速度慢建议是挂梯子（并且设置proxy代理到对应梯子端口），或者采用国内镜像代理。