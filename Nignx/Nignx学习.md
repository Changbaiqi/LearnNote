---
title: Nginx学习
date: 2024-03-02 21:12:10
author: 长白崎
categories:
  - "Nginx"
tags:
  - "Nginx"
---

# Nginx学习

---





常用指令：

停止

systemctl stop nginx





重启

systemctl restart nginx







## 关于nginx总是403问题：

说明：

> 如果使用的是Centos那么并且防火墙啥的都关了可能存在SELinux的问题，只需要把SELinux关了就行
>
> centos关闭SELinux指令：
>
> ```shell
> sed -i s#SELINUX=enforcing#SELINUX=disabled# /etc/selinux/config
> ```
>
> 
