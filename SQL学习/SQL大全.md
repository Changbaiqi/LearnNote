---
title: 常用SQL大全
date: 2023-03-22 17:42:35
author: 长白崎
categories:
  - ["数据库","SQL"]
tags:
  - "SQL"
---



# 常用SQL大全

---

```sql
#显示查询数据库文件存储所在磁盘位置
show variables like '%datadir%';
```

```sql
#日志文件路径
show variables like 'general_log_file';
#错误日志文件路径
show variables like 'log_error';
#慢查询日志文件路径
show variables like 'slow_query_log_file';
```



修改数据库密码：

```sql
set password for 'root'@'%' = password('123456');
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

> 这里指的是更新用户名称为root其访问范围为%的账号密码为123456，注意%代表的是无任何访问限制的账户也就是说可以从公网访问，如果%替换为localhost，那么意思就是只能数据库所在机子才能访问。



删除数据库：

```sql
DROP USER 'root'@'localhost';
```

> 这里演示的是删除root账户



刷新权限：

```sql
flush privileges;
```

