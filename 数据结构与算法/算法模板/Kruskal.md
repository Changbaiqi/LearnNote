---
title: Kruskal算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "算法"
  - "Kruskal"
---



# Kruskal

---

## 介绍：

> Kruskal算法是一种基于图的算法，他的主要作用就是找出图中的最小生成树，这个算法会在一些比如铺设电网最小开销等问题上有相应的应用。

## Java代码模板：

> ```java
> 
> /**
>  * @description: TODO
>  * @author 长白崎
>  * @date 2023/3/25 18:00
>  * @version 1.0
>  */
> 
> import java.util.ArrayList;
> import java.util.Arrays;
> import java.util.Random;
> 
> /**
>  * Kruskal算法的思想其实很简单，但是了其核心难点在于如何查出闭环，而查闭环这一步骤需要用到一个算法，那就是并查集算法，如果还没有了解过并查集算法的同学可以先去了解一下
>  * 以下是Kruskal算法的主要步骤：
>  * 1、首先就是先整理好所有的结点，这里的话比如把所有结点都加入到一个数组或者一个集合里面,但是这里一定要注意，那就是数组或集合对应的下标一定要对应相应的Point类里面的table标签值，不然会出问题。
>  * 2、用一个集合存所有边，也就是题目里面输入的所有点到点的边
>  * (这里一定要特别注意！！！！！边的起点和结束点标签一定要有规律，比如你要么起始结点的标签比结束标签的结点大，要么小，千万别搞混了)
>  * 3、对所有的边基于他的长度或者说成本进行从小到达排序
>  * 4、基于第三步拿到排序后的边的集合后逐一遍历选取边进行连接。当然，这里要主要在进行连接的同时要先检验所选边是否会形成回路，这里就要用到并查集算法了
>  */
> public class Main {
> 
>     public static void main(String[] args) {
> 
>         //以下为测试集，不用理睬。。。。。。
>         Point point[] = new Point[10];
>         point[0] = new Point(1,3,0);
>         point[1] = new Point(4,7,1);
>         point[2] = new Point(3,10,2);
>         point[3] = new Point(9,4,3);
>         point[4] = new Point(12,2,4);
>         point[5] = new Point(11,2,5);
>         point[6] = new Point(12,5,6);
>         point[7] = new Point(23,7,7);
>         point[8] = new Point(1,2,8);
>         point[9] = new Point(6,8,9);
> 
>         E e[] = new E[9+8+7+6+5+4+3+2+1];
>         for(int y= 0,i=0; y < point.length ; ++y){
>             for(int x = y+1; x < point.length ; ++x,++i){
>                 e[i] = new E(point[y],point[x], (int) (Math.random()*100+1));
>             }
>         }
> 
> 
> 
>         kruskal(point,e);
> 
>     }
> 
> 
> 
>     public static void kruskal(Point point[]/*所有的点*/,E e[]/*所有的边*/){
> 
>         //定制化排序对存边的数组基于他的长度进行从小到大排序。
>         Arrays.sort(e,(a,b)-> a.getLen()-b.getLen());
> 
>         //并查集用的数组，下标代表子结点，对应下标存的内容代表父结点
>         int parent[] = new int[point.length];
>         for(int i = 0 ; i< parent.length ; ++i) parent[i] =i; //初始化并查集的数组
> 
>         ArrayList<E> selectE = new ArrayList<>(); //这个用来存选的边
> 
>         for(int i= 0 ; i< e.length ; ++i){
>             E ne = e[i];
>             int startRoot = find(parent,ne.getStartPoint().getTable()); //查询起始结点的根结点
>             int endRoot = find(parent,ne.getEndPoint().getTable()); //查询结束结点的根结点
>             //如果其实结点和结束的结点的根结点是同一个，那么说明会形成闭环，那就不选这条边
>             if(startRoot == endRoot)
>                 continue;
> 
>             //选择该边
>             parent[startRoot] = endRoot; // 更新并查集父子结点关系
>             selectE.add(ne); //将边加入到选择的集合当中
>         }
> 
>         int ans = 0;
>         for(int i =0 ; i < selectE.size(); ++i){
>             System.out.println(selectE.get(i).getStartPoint().getTable()+"——"+selectE.get(i).getEndPoint().getTable());
>             ans+= selectE.get(i).getLen();
>         }
>         System.out.println("最小生成树的边总长度大小："+ans);
>     }
> 
>     /**
>      * 并查集核心算法
>      * @param parent 代表的是并查集用的数组
>      * @param point 代表的是需要查的结点的根结点
>      * @return
>      */
>     public static int find(int parent[],int point){
>         while(parent[point]!=point)
>             point = parent[point];
>         return point;
>     }
> }
> 
> /**
>  * 这个类代表的是一个结点，一个节点里面所包含的信息有这个点的x坐标和y坐标
>  */
> class Point{
>     private int x; //代表点的x坐标
>     private int y; //代表点的y坐标
> 
>     private int table; //代表结点的标签，比如结点第一个那这里就是1，第二个结点那么就是2
> 
>     public Point(int x, int y,int table) {
>         this.x = x;
>         this.y = y;
>         this.table = table;
>     }
> 
>     public int getX() {
>         return x;
>     }
> 
>     public void setX(int x) {
>         this.x = x;
>     }
> 
>     public int getY() {
>         return y;
>     }
> 
>     public void setY(int y) {
>         this.y = y;
>     }
> 
>     public int getTable() {
>         return table;
>     }
> 
>     public void setTable(int table) {
>         this.table = table;
>     }
> }
> 
> /**
>  * 这个类代表的是边，也就是两节点之间的一条连接的边
>  */
> class E{
>     Point startPoint; //边的起始结点
>     Point endPoint; //边的结束结点
> 
>     int len; //边的长度
> 
>     public E(Point startPoint, Point endPoint, int len) {
>         this.startPoint = startPoint;
>         this.endPoint = endPoint;
>         this.len = len;
>     }
> 
>     public Point getStartPoint() {
>         return startPoint;
>     }
> 
>     public void setStartPoint(Point startPoint) {
>         this.startPoint = startPoint;
>     }
> 
>     public Point getEndPoint() {
>         return endPoint;
>     }
> 
>     public void setEndPoint(Point endPoint) {
>         this.endPoint = endPoint;
>     }
> 
>     public int getLen() {
>         return len;
>     }
> 
>     public void setLen(int len) {
>         this.len = len;
>     }
> }
> ```