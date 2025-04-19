---
title: ActiveMQ学习
date: 2025-04-18 15:18:00
author: 长白崎
categories:
  - ["ActiveMQ"]
tags:
  - "Java"
  - "ActiveMQ"
---



# ActiveMQ学习

---

1.解压activemq压缩包，重命名文件夹

```shell
tar -zxvf apache-activemq-5.11.1-bin.tar.gz
mv apache-activemq-5.11.1 activemq
```

2.防火墙开启activemq端口8161（管理平台端口）和61616（通讯端口）

```shell
vi /etc/sysconfig/iptables
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8161 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 61616 -j ACCEPT
```

3.启动、访问、查看状态和停止activemq服务

```shell
./activemq/bin/activemq start
wget 192.168.2.137:8161
./activemq/bin/activemq status
./activemq/bin/activemq stop
```

二、activemq注册为系统服务，开启开机自启

1.建立软连接

```shell
ln -s /usr/local/activemq/bin/activemq /etc/init.d/ #路径根据实际情况修改
```

2.注册为系统服务

```shell
vi /etc/init.d/activemq
# 添加下面内容到/etc/init.d/activemq脚本
chkconfig: 345 63 37
description: Auto start ActiveMQ
JAVA_HOME=/usr/local/jdk1.8.0_144
JAVA_CMD=java
```



3.开启开机自启

```shell
chkconfig activemq on
```

4.可以以系统服务的方式启动、查看状态和停止服务

```shell
service activemq start
service activemq status
service activemq stop
```

