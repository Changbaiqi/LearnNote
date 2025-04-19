---
title: Dijkstra算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "Dijkstra"
---



# Dijkstra

---

## 说明：

> Dijkstra算法是一种求图的单元最短路径的算法
>
> 参考视屏：https://www.bilibili.com/video/BV1zz4y1m7Nq

## Java代码模板（基于链式向前星，Dijkstra算法时间复杂度O(nlogn)）：

> ```java
> 
> import java.util.Arrays;
> import java.util.PriorityQueue;
> import java.util.Scanner;
> 
> /**
>  * Dijkstra模板
>  * @author 长白崎
>  *
>  */
> /*
> 6 9
> 1 5 4
> 1 6 1
> 2 4 6
> 2 5 5
> 2 6 4
> 3 4 2
> 3 5 3
> 4 5 2
> 4 6 7
> 1
>  */
> public class Dijkstra {
> 	static int cnt = 0;
> 	final static int inf = 0x3f3f3f3f;
> 	public static void main(String[] args) {
> 		// TODO Auto-generated method stub
> 		Scanner sc = new Scanner(System.in);
> 		int n = sc.nextInt(); //n个点
> 		int m = sc.nextInt(); //m条边
> 		
> 		int head[] = new int[n+1]; //链式向前星头结点
> 		init(head); //初始化头结点
> 		Edge e[] = new Edge[m*2+1];
> 		while(m-->0) {
> 			int u = sc.nextInt();
> 			int v = sc.nextInt();
> 			int w = sc.nextInt();
> 			addEdge(head, e, u, v, w);//添加边，增加一次代表有向图u->v;
> 			addEdge(head, e, v, u, w); //点对换再增加一次相当于无向图了
> 		}
> 		
> 		int start = sc.nextInt();
> 		int dis[] = dijkstra(head, e,start);
> 		for(int i=1 ;i<dis.length;++i) {
> 			System.out.printf("%d -> %d 最短距离为： %d\n",start,i,dis[i]);
> 		}
> 		
> 	}
> 	
> 	/**
> 	 * Dijkstra核心算法
> 	 * @param head 链式向前的头结点数组
> 	 * @param e 链式向前星的边寄存数组
> 	 * @param start 起点
> 	 * @return 返回start起点到所有其他点的最小距离数组
> 	 */
> 	public static int[] dijkstra(int head[],Edge e[],int start) {
> 		int dis[]=new int[head.length]; Arrays.fill(dis, inf); //创建存储距离的数组并且全都初始化为无穷距离
> 		boolean st[] = new boolean[head.length]; //标记是否走过该点
> 		
> 		//优先队列并且按照权值从小到大排列
> 		PriorityQueue<Node> qu = new PriorityQueue<Node>((a,b)-> a.w-b.w);
> 		
> 		//添加起点到优先队列，因为是起点，所以起点的总距离为0
> 		qu.offer(new Node(start,0));dis[start]=0;
> 		while(!qu.isEmpty()) {
> 			Node u = qu.poll(); //出队，u.u代表是以u为起点，u.w代表以u.u为起点时从start起点到u.u点的最短距离（优先队列性质）
> 			
> 			if(st[u.u]) continue; //如果该点走过了那么就直接跳过
> 			st[u.u] = true; //表示没走过现在开始走，并且先标记为走过
> 			
> 			//循环遍历以u.u为起点的所有边
> 			for(int i =head[u.u];i!=-1; i=e[i].next) {
> 				Edge v = e[i]; //下一步要走的终点
> 				if(u.w+v.w>dis[v.to] || st[v.to])continue; //如果大于寄存在dis数组里面的最小距离或者选择的点已经走过了那么直接continue即可
> 				dis[v.to] = u.w+v.w;  //更新dis中的start起点到v.to点的最短距离
> 				qu.offer(new Node(v.to,dis[v.to])); //添加到优先队列里面
> 			}
> 			
> 		}
> 		return dis;
> 	}
> 
> 	/**
> 	 * 初始化链式向前星的头结点,-1代表结尾
> 	 * @param head 链式向前星头结点数组
> 	 */
> 	static void init(int head[]) {Arrays.fill(head, -1);}
> 	/**
> 	 * 链式向前星添加边
> 	 * @param u 弧尾
> 	 * @param v 弧头
> 	 * @param w 权值
> 	 */
> 	static void addEdge(int head[],Edge e[],int u,int v,int w) { e[cnt]=new Edge(v,head[u],w);head[u]=cnt++;}
> 	
> 	static class Edge{
> 		int to,next,w;
> 		public Edge(int to,int next,int w ) {
> 			this.to = to; this.next = next;this.w = w;
> 		}
> 	}
> 	
> 	static class Node{
> 		int u,w;
> 		public Node(int u,int w) {this.u =u;this.w = w;}
> 	}
> 
> }
> 
> ```

