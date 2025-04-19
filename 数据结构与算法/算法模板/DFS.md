---
title: DFS算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "算法"
  - "DFS"
---



# DFS

---

## 介绍：

> DFS中文叫做深度优先搜索，DFS算是暴力搜索的其中一种算法，这个算法主要还是可以解决一些最小路径的问题，以及搜索问题，比如迷宫问题等等，其主要思想就是通过穷举所有可能走的路并找到答案或者试出最优答案。

## Java代码模板：

> ```java
> 
> /**
>  * @description: TODO
>  * @author 长白崎
>  * @date 2023/3/25 16:34
>  * @version 1.0
>  */
> 
> /**
>  * DFS算法模板，DFS算法的模板写法主要分为这几步骤：
>  * 1、判断是否到达要求条件
>  * 2、穷举所有可能走的方向
>  * 3、通过第2步穷举的方向然后去走，当然走之前还要过滤那些不合格的方向，比如这这个方向的下一步走过了，不能再走了，或者这个方向的下一步有墙也不能走等，
>  *    实际的拦截条件根据题目要求添加。
>  */
> public class Main {
>     static int minStep =Integer.MIN_VALUE; /*这个变量用于存储走到终点最少需要多少步*/
>     public static void main(String[] args) {
>         int map[][] = new int[20][20]; //这个数组就是地图
>         int state[][] = new int[map.length][map[0].length]; //这个数组充当标记走过的路的数组
> 
> 
>     }
> 
>     public static void dfs(int map[][]/*传入地图,0为可以走，1为墙*/,int state[][]/*传入标记走过的状态数组，0为未走过，1为走过*/,int nx/*下一步要走的x坐标*/,int ny/*下一步要走的y坐标*/,int ex/*终点的x坐标*/,int ey/*终点的y坐标*/,int step/*当前步数*/){
> 
>         if(nx==ex && ny==ey){
>             minStep = Math.min(minStep,step);
>             return;
>         }
> 
>         int next[][]={
>                 {nx,ny-1},/*上*/
>                 {nx,ny+1},/*下*/
>                 {nx-1,ny},/*左*/
>                 {nx+1,ny}/*右*/
>         };
> 
>         for(int i= 0; i < next.length ; ++i){
>             //过滤越界
>             if(next[i][0]<0 || next[i][1]<0 || next[i][1]>=map.length || next[i][0] >=map[0].length)
>                 continue;
>             //过滤墙
>             if(map[next[i][1]][next[i][0]]==1)
>                 continue;
>             //过滤走过的情况
>             if (state[next[i][1]][next[i][0]]==1)
>                 continue;
>             //其他过滤条件根据题目要求可以持续添加或删减
>             //......
> 
>             state[next[i][1]][next[i][0]] =1; //标记为走过
>             dfs(map,state,next[i][0],next[i][1],ex,ey,step+1);
>             state[next[i][1]][next[i][0]] =0;//取消标记
>         }
>     }
> }
> ```