---
title: Dijkstra算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - "数据结构与算法"
tags:
  - "Dijkstra"
---



# Dijkstra

---

## 说明：

> Dijkstra算法是一种求图的单元最短路径的算法
>
> 参考视屏：https://www.bilibili.com/video/BV1zz4y1m7Nq

## Java代码模板：

> ```java
> 
> import java.util.*;
> 
> /**
>  * @description: Dijkstra算法
>  * Dijkstra用于算法用于求图的最小单元路径
>  * @author 长白崎
>  * @date 2023/4/5 16:40
>  * @version 1.0
>  */
> public class Dijkstra {
>     public static void main(String[] args) {
> 
>         //这部分为测试用的数据集
>         int map[][] = new int[5][5];
> 
>         map[0][1] =2;
>         map[1][0] = 2;
> 
>         map[0][2] =4;
>         map[2][0] =4;
> 
>         map[1][3] = 5;
>         map[3][1] = 5;
> 
>         map[1][2] = 6;
>         map[2][1] = 6;
> 
>         map[2][3] = 1;
>         map[3][2] = 1;
> 
>         map[2][4] = 3;
>         map[4][2] = 3;
> 
>         map[3][4] =8;
>         map[4][3] =8;
> 
> 
> 
>         dijkstra(map,0,4);
>     }
> 
>     /**
>      * Dijkstra核心算法
>      * @param map 地图
>      * @param start 起始点
>      * @param end 终点
>      */
>     public static void dijkstra(int map[][],int start,int end){
> 
>         //用于寄存最小距离
>         int dis[] = new int[map.length];
>         //用于寄存父结点
>         int parent[] = new int[map.length];
>         //标记是否选择了
>         boolean select[] = new boolean[map.length];
> 
>         //初始化距离全为最大值
>         Arrays.fill(dis,Integer.MAX_VALUE);
>         //初始化父结点为自身
>         for(int i = 0 ; i <parent.length ; ++i)
>             parent[i] = i;
> 
>         //初始化起点
>         dis[start] = 0; //自己到自己的距离为0
>         select[start] = true; //标记为选择了
> 
>         //代表当前所选的点
>         int point  = start;
>         while(true){
>             int resPoint = -1; //临时存储所选点
>             int resDis = Integer.MAX_VALUE; //临时存储所选边的长度
> 
>             for(int i= 0; i < map.length ; ++i){
>                 //过滤已选点
>                 if(select[i])
>                     continue;
> 
>                 //过滤题目要求，比如距离为0的点表示不连通
>                 if(map[point][i]==0)
>                     continue;
>                 //。。。。
> 
>                 //判断是否总值是否小于当面边的距离
>                 if(dis[i]<map[i][point]+dis[point])
>                     continue;
> 
>                 //更新距离
>                 dis[i] = map[i][point]+dis[point];
>                 parent[i] = point;
>             }
> 
>             //找最小边
>             for(int i = 0; i < select.length ; ++i){
>                 //过滤已选边
>                 if(select[i])
>                     continue;
>                 //过滤比单当前大的值
>                 if(resDis<dis[i])
>                     continue;
> 
>                 resPoint =i;
>                 resDis = dis[i];
>             }
>             //选择最小边
>             if(resPoint!=-1){
>                 point = resPoint;
>                 select[point]=true;
>             }
> 
>             //如果已经搜寻到终点那么就退出
>             if(point==end)
>                 break;
>         }
>         //输出最小距离
>         System.out.println(dis[end]);
>         //输出回溯路径
>         pr(parent,end);
>     }
> 
>     //这一部分用于回溯路径
>     public static void pr(int parent[],int end){
>         while(end!=parent[end]){
>             System.out.print(end+"<-");
>             end = parent[end];
>         }
>         System.out.print(end);
>     }
> 
> }
> ```

