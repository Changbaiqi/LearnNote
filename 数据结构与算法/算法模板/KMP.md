---
title: KMP算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - "数据结构与算法"
tags:
  - "算法"
  - "KMP"
---



# KMP

---

## 说明：

> KMP算法其实主要的作用就是匹配子字符串，就比如：
>
> 父串：aabacdca
>
> 子串：acd
>
> 这里我们要查找父串中是否有子串，那么就可以用KMP算法。
>
> 而KMP算法其实有点像一个状态机，或者说一个dp？
>
> 其主要特点还是记录转态回溯。
>
> KMP主要难点在于其中next数组的计算。next数组的计算方式是“前缀和后缀公共部分的最大长度”。比如一个字符串ababa，他的前缀是可以是a，ab，aba，abab（不包含最后一位），后缀是a，ba，aba，baba（不包含第一位）。
>
> 

## Java代码模板：

> ```java
> 
> /**
>  * @description: KMP算法
>  * KMP是一种字符串匹配算法，他是一种时间复杂度低，比较优秀的一种字符串匹配算法，一般可以拿来做一下字符串匹配类的题目。他与一般的暴力匹配比较其有点就是
>  * 有记录和回溯子串的功能，相比普通的暴力匹配减少了很多没必要的匹配次数，其思想概念有点类似于dp，和简单状态机。
>  * @author 长白崎
>  * @date 2023/3/28 0:28
>  * @version 1.0
>  */
> import java.util.Arrays;
> import java.util.Scanner;
> 
> public class KMP {
> 
> 	public static void main(String[] args) {
> 		// TODO Auto-generated method stub
> 		Scanner sc = new Scanner(System.in);
> 		
> 		//String s = sc.next();
> 		String pat = sc.next();
> 		
> 		int next[] = new int[pat.length()];
> 		
> 		buildNext(pat.toCharArray(), next);
> 		System.out.println(Arrays.toString(next));
> 		//System.out.println(kmp(s.toCharArray(),pat.toCharArray(),next));
> 		
> 	}
> 	
> 	
> 	
> 	public static int kmp(char s[],char pat[],int next[]) {
> 		
> 		
> 		int j=0;
> 		for(int i=0 ; i<s.length ; ++i) {
> 			while(j>0 && s[i]!=pat[j]) j =  next[j-1];
> 			if(s[i]==pat[j]) ++j;
> 			if(j==pat.length) return i-pat.length+1;
> 		}
> 		return -1;
> 	}
> 	
> 	public static void buildNext(char pat[],int next[]) {
> 		
> 		int i = 0;
> 		for(int j=1; j < pat.length ; ++j) {
> 			while(i>0 && pat[i]!=pat[j]) i = next[i-1];
> 			if(pat[i]==pat[j]) ++i;
> 			next[j] = i;
> 		}
> 		
> 	}
> 
> }
> ```