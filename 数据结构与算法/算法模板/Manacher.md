---
title: Manacher算法模板
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - "数据结构与算法"
tags:
  - "算法"
  - "Manacher"
---



# Manacher

---

## 说明：

> Manacher算法主要是用于字符串查找最长回文子串，其原理就是在原本的字符串插入其他特殊符号达到把我们所有需要所有需要匹配的回文都变成了奇回文，并且以其奇回文的中点向左右扩散计算其回文长度。

## Java代码模板：

> ```java
> 
> import java.util.Arrays;
> 
> /**
>  * @description: 马拉车（Manacher）算法模板
>  * 此算法是一种匹配子串最长回文算法，可以大大减少时间复杂度，但是这里一定要主要要使用数组，不能直接使用字符串拼接，字符串直接拼接效率相比数组使用
>  * 效率会非常差，比较长的回文会超时。
>  * @author 长白崎
>  * @date 2023/6/1 22:39
>  * @version 1.0
>  */
> public class Manacher算法 {
> 
>     public static void main(String[] args) {
>         //测试数据
>         char text1[] = "aba".toCharArray();
>         char text2[] = "abcba".toCharArray();
>         char text3[] = "abracbalnavlvnalvnaslfslajslauogaslga".toCharArray();
>         char text4[] = "aa1ABA1b".toCharArray();
> 
>         manacher(text1);
>         manacher(text2);
>         manacher(text3);
>         manacher(text4);
>     }
>     public static void manacher(char text[]){
>         /*
>         * 这里设置成len*2+1+2主要是开辟一个数组用于寄存我们需要判断的回文字符串
>         * 比如：^#a#b#a#* 需要开原长度的*2+1+2也就是3*2+1+2=9
>         * ^#a#b#c#b#a#* 需要开原长度的*2+1+2也就是5*2+1+2=13
>         */
>         char resText[] = new char[text.length*2+1+2];
> 
> 
>         resText[0]='^';
>         resText[1]='#';
>         for(int i= 0; i <text.length ; ++i){
>             resText[i*2+2] = text[i];
>             resText[i*2+2+1] = '#';
>         }
>         resText[resText.length-1] = '*';
> 
>         //用于存储以i为中心的回文长度
>         int ans[] = new int[resText.length];
> 
>         int i= 1;
>         int maxLen = 0; //这个是用于记录最长回文长度
>         while(i<resText.length-2){
>             int l = i-1;
>             int r= i+1;
>             int len =0;
>             while(resText[l]==resText[r]){
>                 ++len;
>                 --l;
>                 ++r;
>             }
> 
>             ans[i] = len;
>             //用于比较赋值最长回文
>             maxLen = maxLen<len ? len : maxLen;
>             ++i;
>         }
> 
>         System.out.println("最长回文长度为："+maxLen);
>         System.out.println("回文的数组："+pr(ans));
>     }
> 
>     public static String pr(int data[]){
>         return Arrays.toString(data);
>     }
> 
> }
> 
> ```