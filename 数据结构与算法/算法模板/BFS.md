---
title: BFS算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "算法"
  - "BFS"
---



# BFS

---

## 介绍：

> BFS中文叫做广度优先搜索，BFS算是暴力搜索的其中一种算法，这个算法主要还是可以解决一些最小路径的问题，以及搜索问题，比如迷宫问题等等，其主要思想就是通过穷举所有可能走的路并找到答案或者试出最优答案，不过他相对于DFS说其有点就在于广撒网，时间复杂度要比DFS低。

## Java代码模板：

> ```java
> 
> /**
>  * @description: TODO
>  * @author 长白崎
>  * @date 2023/3/25 17:42
>  * @version 1.0
>  */
> 
> 
> import java.util.LinkedList;
> import java.util.Queue;
> 
> /**
>  * BFS算法模板，BFS算法的模板写法主要分为这几步骤：
>  * 1、选择BFS的实现方式，一般有两种，一种是基于队列，一种是基于栈。
>  * 2、判断是否到达要求条件
>  * 3、穷举所有可能走的方向
>  * 4、通过第2步穷举的方向然后去走，当然走之前还要过滤那些不合格的方向，比如这这个方向的下一步走过了，不能再走了，或者这个方向的下一步有墙也不能走等，
>  *    实际的拦截条件根据题目要求添加。
>  * 5、最后将能走的入队或入栈。
>  * 因为BFS的算法的特性，基于队列的BFS第一次所搜索到的路径就是最短路径。
>  * 警告：如果你是想想通过BFS去查找最优步数以及最优路径推荐选择基于队列，基于栈可能出毛病。
>  */
> public class Main {
> 
>     public static void main(String[] args) {
>         int map[][] = new int[20][20]; //这个数组就是地图
>         int state[][] = new int[map.length][map[0].length]; //这个数组充当标记走过的路的数组
> 
>         int minStep = bfs(map,0,0,19,19); //这里返回的值是从起点到终点的最小步数
>     }
> 
> 
>     public static int bfs(int map[][]/*传入地图,0为可以走，1为墙*/,int nx/*起点x坐标*/,int ny/*起点的y坐标*/,int ex/*终点的x坐标*/,int ey/*终点的y坐标*/){
>         Queue<int[]> data = new LinkedList<>(); //用于BFS的队列，这里也可以改成栈
>         int state[][] = new int[map.length][map[0].length]; //用于记录走过状态的数组，0为未走过，1为走过
>         data.add(new int[]{nx,ny,0}); //将起点加入到队列
>         state[ny][nx]=1; //将起点标记为走过
> 
>         int minStep =Integer.MAX_VALUE; //用于记录到达终点的最小步数
>         while(!data.isEmpty()){
>             int point[] = data.poll(); //出队(栈)
> 
>             //判断是否到达终点
>             if(point[0]==ex && point[1]==ey){
>                 minStep = Math.min(point[2],minStep); //更新最小步数
>                 break;
>             }
> 
>             //穷举所有可能走的方向
>             int next[][] ={
>                     {point[0],point[1]-1},//上
>                     {point[0],point[1]+1},//下
>                     {point[0]-1,point[1]},//左
>                     {point[0]+1,point[1]},//右
>             };
> 
>             for(int i= 0 ; i< next.length ; ++i){
>                 //过滤数组越界
>                 if(next[i][0]<0 || next[i][1]<0 || next[i][1]>=map.length || next[i][0]>=map[0].length)
>                     continue;
>                 //过滤障碍物，比如墙
>                 if(map[next[i][1]][next[i][0]]==1)
>                     continue;
>                 //过滤走过的格子
>                 if(state[next[i][1]][next[i][0]]==1)
>                     continue;
> 
>                 //其他附加过滤条件可以根据题目要求做调整
>                 //......
> 
>                 state[next[i][1]][next[i][0]]=1; //将即将要走的格子标记为走过
>                 data.offer(new int[]{next[i][0],next[i][1],point[2]+1}); //将要走的格子加入到队列
>             }
>         }
>         return minStep;
>     }
> }
> ```