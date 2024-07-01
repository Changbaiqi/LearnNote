---
title: Nunjucks Error expected variable end解决办法
date: 2024-06-08 15:08:36
author: 长白崎
categories:
  - "Hexo"
tags:
  - "Hexo"
---

# Nunjucks Error expected variable end解决办法

---

{% raw %}

## 问题：

> 如果在部署hexo博客的时候报错Nunjucks Error expected variable end则可能是因为Hexo使用Nunjucks渲染贴子，使用=={{ }}==或者=={% %}==包装的内容被解析，这样会引起冲突。

{% endraw %}

## 解决方法：

使用以下标签包裹对应铭感字段就行：

```PLAINTEXT
{% raw %}

{% endraw %}
```

