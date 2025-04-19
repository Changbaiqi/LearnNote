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

## Java代码模板（时间复杂度O(n)）：

> ```java
> 
> import java.io.BufferedReader;
> import java.io.IOException;
> import java.io.InputStreamReader;
> import java.io.PrintWriter;
> import java.util.StringTokenizer;
> /**
>  * 
>  * @author 长白崎
>  *
>  */
> public class ManacherStudy {
> 	final static PrintWriter out = new PrintWriter(System.out);
> 	
> 	public static void main(String[] args) throws IOException {
> 		// TODO Auto-generated method stub
> 		char str[] = R.next().toCharArray();
> 		
> 		out.println(manacher(str));
> 		out.flush();
> 		out.close();
> 	}
> 	
> 	/**
> 	 * Manacher核心算法
> 	 * @param str 需要进行判断的字符串
> 	 * @return 返回最长回文子串的长度
> 	 */
> 	public static long manacher(char str[]) {
> 		char s[] = new char[str.length*2+2];//Manacher处理过后的字符串
> 		int m = 0; s[++m]='#';
> 		
> 		for(int i=0;i<str.length;++i) {s[++m]=str[i]; s[++m]='#';};
> 		
> 		int p[] = new int[s.length]; //用于寄存p[i]点的回文串长度
> 				//M为最右回文中间值
> 		//M最长回文中间值,R为右边界，max长回文子串
> 		int M = 0,R=0,max=0;
> 		for(int i =1;i<=m;++i) {
> 			
> 			if(i>R) p[i]=1; else p[i]=Math.min(p[2*M-i], R-i+1);
> 			
> 			//(i-p[i])和(i+p[i])是为了防止越界
> 			while(i-p[i]>=1 && i+p[i]<=m && s[i+p[i]]==s[i-p[i]])++p[i];
> 			if(p[i]+i-1>R) {M=i;R=i+p[i]-1;} //如果当前回文串的长度超过之前的最大覆盖回文串的长度那么久替换M和R
> 			max= Math.max(max, p[i]);
> 		}
> 		
> 		
> 		return max-1;
> 	}
> 	
> 	static class R{
> 		static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
> 		static StringTokenizer tk = new StringTokenizer("");
> 		public static String next() throws IOException {
> 			while(!tk.hasMoreTokens()) {
> 				tk = new StringTokenizer(br.readLine());
> 			}
> 			return tk.nextToken();
> 		}
> 	}
> 
> }
> 
> ```



## C++模板(时间复杂度O(n)):

```C++
#include<iostream>
#include<cstring>
#include<cmath>
using namespace std;

const int N = 2.2e7+5;

string n;
char s[N];
int p[N];
int manacher(){
    int m =0;s[++m]='#';
    for(int i=0;i<n.length();++i){s[++m]=n[i];s[++m]='#';}

    int M=0,R=0,mx=0;
    for(int i =1;i<=m;++i){
        if(i>R)p[i]=1;else p[i]= min(p[2*M-i],R-i+1);

        while(i-p[i]>=1 && i+p[i]<=m && s[i-p[i]]==s[i+p[i]]) ++p[i];
        if(i+p[i]-1>R) M=i,R=i+p[i]-1;
        mx=max(mx,p[i]);
    }
    return mx-1;
}
int main(){
    cin >> n;
    cout << manacher()<<endl;

    return 0;
}
```



