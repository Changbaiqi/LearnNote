---
title: BinarySearch
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "算法"
  - "BinarySearch"
---



# BinarySearch

---

## 说明：

> BinarySearch中文又叫做二分查找，这是一种查找类的算法，但是其使用是有一定的限制的，那就是必须要区间类必须要满足相应的单调性，不然的话是无法使用的。

## Java代码模板：

### 整形二分：

> ```java
> //这是一个Java整数二分模板
> public static void binarySearch(){
> 	//l为二分的左值，r为二分的右值，mid为二分的中间值
>     int l=0,r=100,mid;
>     //这里的l<=r为二分的结束条件
>     while(l<=r){
>         //计算二分的mid
>         mid = (l+r)>>1;
>         //这里的check函数主要的作用就是通过已知的必要条件传入check进行综合分析然后判断应该之后的二分是右移还是左移
>         if(check(Object c))
>             l = mid+1; //这里是右移
>         else
>             r = mid-1; //这里是左移
>     }
> }
> 
> 
> //这个是check函数，check函数的书写需要结合实际来
> public static void check(Object c){
>     //......
> }
> ```

### 高精度二分：

> ```java
> //这个是Java的高精度二分
> public static void BianrySearch(){
>     //l为这里的左值，r为这里的右值，mid为中值
>     double l = 0,r=1e11,mid;
>     //这里不在是和之前整形二分一样的l<r了，这里因为高精度的特性所以必须要写成右值-左值>0.000001，这里的0.000001可以多添一些0别太少就行
> 	while(r-l>1e-5) {
> 		mid = (l+r)/2.0;
> 		//这里是check函数，负责根据必要条件镜进行判断是右移还是左移
>         if(check(Object o))
> 			l = mid;//右移
> 		else
> 			r =mid;//左移
> 	}
> }
> //check函数
> public static void check(){
>     
> }
> ```



## C++代码模板

### 整形二分

> ```c++
> #include<iostream>
> 
> using namespace std;
> 
> int main(){
>     //l为二分的左值，r为二分的右值，mid为二分的中间值
>     int l = 0,r = 10000,mid=0;
>     while(l<r){
>         mid = (r+l+1)>>1;     //计算二分的mid
>         //这里的check函数主要的作用就是通过已知的必要条件传入check进行综合分析然后判断应该之后的二分是右移还是左移
>         if(check(mid))
>             l = mid;//右移
>         else
>             r = mid-1; //左移
>     }
>     
>     return 0;
> }
> 
> //check函数
> bool check(int mid){
>     ...
>     return false;
> }
> ```

