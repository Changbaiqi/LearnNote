---
title: Mysql中SQL的执行顺序
date: 2025-03-02 17:42:35
author: 长白崎
categories:
  - ["数据库","SQL"]
tags:
  - "SQL"
---

# Mysql中SQL的执行顺序

---

MySQL语句的执行顺序与日常书写顺序不同，其核心逻辑是从数据源逐步过滤、聚合到最终输出，整个过程会生成多个中间虚拟表。一下是关键执行步骤及说明：

## 一、SELECT语句的执行顺序

1.FROM & JOIN

首先确定数据库来源，处理多表连接如（LEFT JOIN、INNER JOIN）。若涉及多个表，会按顺序生成笛卡尔积并逐步过滤。

示例：`FROM a JOIN b ON a.id=b.a_id`会先连接a和b表。

2.ON

应用连接条件，筛选出符合逻辑的行，生成中间表（如VT2）。

注意：ON条件用于连接操作，而`WHERE`用于最终过滤操。

3.WHERE

对于连接后的结果进行条件过滤（如WHERE age >18）,生成VT4。

关键点：`WHERE`过滤的是原始数据，而非分组后的数据。

4.GROUP BY

按指定列分组（如`GROUP BY department`），生成VT5。分组后每组仅保留一行。

注意：非聚合列必须包含在`GROUP BY`中。

5.HAVING

对分组后的结果进行过滤（如`HAVING avg_salary > 5000`），生成VT7。

与WHERE的区别：`HAVING`作用于聚合后的数据。

6.SELECT

选择最终输出的列（如`SELECT name,SUM(salary)`)，生成VT8。

7.ORDER BY

对结果排序（如ORDER BY total DESC），生成VT10。排序操作消耗资源较多，建议谨慎使用。

8.LIMIT

限制返回行数（如LIMIT 10），生成最终结果集并返回给客户端。



## 二、更新语句的执行顺序

更新（UPDATA）和删除（DELETE）语句的执行流程类似：

1.连接器：验证用户权限。

2.分析器：解析SQL语法和语义。

3. 优化器：生成执行计划（如选择索引）。
4. 执行器：调用存储引擎接口，执行具体操作并返回结果。



## 三、关键细节与优化建议

1. 中间表与性能

每个步骤生成虚拟表，数据量逐步减少。建议通过WHERE提前过滤数据，减少后续处理量。

示例：WHERE age > 18比HAVING age > 18更高效。



2. 索引与查询优化

· WHERE和JOIN条件列需建立索引。

· 避免在SELECT中进行复杂计算，可延迟到应用层处理。



3. 别名与作用域

SELECT中定义的别名（如total）仅能在ORDER BY中使用，不可在WHERE或GROUP BY中引用。



## 四、总结

MySQL通过分步处理+中间表实现查询，理解执行顺序有助于编写高效SQL。例如：

· 优先使用WHERE而非HAVING过滤数据。

· 合理设计索引，减少全表扫描。

· 避免SELECT *，仅查询必要字段。



通过掌握这些规则，可显著提升查询性能并规避常见错误。