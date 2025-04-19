---
title: MySQL学习笔记
date: 2023-02-24 09:46:09
author: 长白崎
categories:
  - "MySql"
tags:
  - "MySql"
---



# MySQL学习笔记

***

> auto_increment ---自增序列
>
> 在插入时，如果不给定具体用户编号，此时根据auto_increment的值递增添加





## 安装Mysql8(CentOS7)

### **一、添加MySQL Yum仓库**

1. **下载MySQL RPM包**

   ```shell
   wget https://dev.mysql.com/get/mysql80-community-release-el7-9.noarch.rpm
   ```

2. **安装MySQL仓库**

   ```shell
   sudo rpm -ivh mysql80-community-release-el7-9.noarch.rpm
   ```

3. **验证仓库是否添加成功**

   ```shell
   yum repolist enabled | grep "mysql.*-community.*"
   ```

------

### **二、安装MySQL 8**

1. **更新系统并安装MySQL**

   ```shell
   sudo yum update
   sudo yum -y install mysql-server --nogpgcheck
   ```

   或

   ```shell
   sudo yum update
   sudo yum -y install mysql-community-server --nogpgcheck
   ```

   

2. **启动MySQL服务**

   ```shell
   sudo systemctl start mysqld
   ```

3. **设置开机自启**

   ```shell
   sudo systemctl enable mysqld
   ```
   
4. **获取初次启动密码**

```shell
grep 'A temporary password' /var/log/mysqld.log
```

### **三、登录MySQL**

```shell
mysql -u root -p
# 输入新设置的密码
```

### **四、第一次登录可能需要修改默认密码操作**

```sql
alter user 'root'@'localhost' IDENTIFIED BY '修改的密码';
```



------

### **五、配置远程访问（可选）**

1. **登录MySQL后执行**

   ```sql
   CREATE USER '用户名'@'%' IDENTIFIED BY '强密码';
   GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```



## 安装Mysql5(CentOS7)



### **一、添加MySQL Yum仓库**

1. **下载MySQL RPM包**

   ```shell
   sudo wget wget https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
   ```

2. **安装MySQL仓库**

   ```shell
   sudo rpm -ivh mysql57-community-release-el7-8.noarch.rpm
   ```

3. **验证仓库是否添加成功**

   ```shell
   yum repolist enabled | grep "mysql.*-community.*"
   ```

------

### **二、安装MySQL 5**

1. **更新系统并安装MySQL**

   ```shell
   sudo yum update
   sudo yum -y install mysql-server --nogpgcheck
   ```

   或

   ```shell
   sudo yum update
   sudo yum -y install mysql-community-server --nogpgcheck
   ```

   

2. **启动MySQL服务**

   ```shell
   sudo systemctl start mysqld
   ```

3. **设置开机自启**

   ```shell
   sudo systemctl enable mysqld
   ```

4. **获取初次启动密码**

```shell
grep 'A temporary password' /var/log/mysqld.log
```

### **三、登录MySQL**

```shell
mysql -u root -p
# 输入新设置的密码
```

### **四、第一次登录可能需要修改默认密码操作**

```sql
alter user 'root'@'localhost' IDENTIFIED BY '修改的密码';
```

若报错`Your password does not satisfy the current policy requirements`则表示该版本Mysql有密码强度检测，一般不允许过于简单的密码，此时如果你还是想搞简单的密码那么可以执行：

```sql
set global validate_password_policy=0;
```



------

### **五、配置远程访问（可选）**

1. **登录MySQL后执行**

   ```sql
   CREATE USER '用户名'@'%' IDENTIFIED BY '强密码';
   GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```



## 彻底删除MySQL(CentOS7)

1.查询本机安装的mysql

```shell
rpm -qa |grep -i mysql
```

2.使用yum remove ..删除每一个安装

删除后在使用rpm -qa |grep -i mysql进行检验

3.查找mysql相关目录

```shell
find / -name mysql
```

4.对这些目录一个个的进行删除

```shell
rm -rf
```

5.删除/etc/my.cnf文件

```shell
rm -rf /etc/my.cnf
```

6.删除/var/log/mysql/mysqld.log文件

```shell
rm -rf /var/log/mysql/mysqld.log
```



## 添加数据库用户

```sql
create user ccc identified by 'pass';
```

上面的命令创建了用户**ccc**，密码是**pass**，在mysql.user里可以查看到新增用户的信息：

```sql
select User,Host from mysql.user where User= 'ccc';
```



## 给予用户数据库权限

```sql
grant all privileges on cccdb.* to ccc@'%';
flush privileges; #刷新权限
```

上面的语句将cccdb数据库的所有操作权限都授权给了用户ccc。

可以通过`show grants`命令查看权限授予执行的命令：

```sql
show grants for 'ccc';
```

