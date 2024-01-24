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

