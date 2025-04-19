---
title: 将数字N拆分为K个不同的数字
date: 2023-06-06 23:10:10
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "算法"
  - "数论"
---



# 将数字N拆分为K个不同的数字

---

## 说明：

> 将数字N拆分为K个不同的数字一共有多少种不同的方法？

## Java代码模板：

```java

public class 数字拆分 {

    public static void main(String[] args) {
        System.out.println(slove(2022,10));
    }

    /**
     * 将数字N拆分为K个不相同的正整数之和，一共有多少种不同的方法？
     * @param num 需要拆分的数字
     * @param k 需要拆分为多少个
     * @return
     */
    public static long slove(int num,int k){
        long bp[][] = new long[num+1][k+1];
        bp[0][0]=1;
        for(int i =1 ; i <= num ; ++i){
            for(int j =1; j <=k ; ++j){
                if(i>=j)
                    bp[i][j] = bp[i-j][j] + bp[i-j][j-1];
            }
        }

        return bp[num][k];
    }
}

```

