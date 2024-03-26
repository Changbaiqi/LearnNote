---
title: Java大数字运算使用技巧
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["Java"]
  - ["数据结构与算法"]
tags:
  - "Java"
  - "算法"
  - "大数字运算"
---



# Java大数字运算使用技巧

---

## 说明：

> Java大数字使用技巧,这里只演示BigInteger了，BigDecimal高精度大数字就不演示了(用法基本一样)。

## Java代码模板

> ```java
> 
> import java.math.BigInteger;
> 
> public class BigNumber {
> 
>     public static void main(String[] args) {
>         
>     }
> 
> 
> 
>     /**
>      * 进制转换
>      */
>     public void testScale() {
>         
>         //在构造将函数时，把radix进制的字符串转化为BigInteger
>         String str = "1011100111";
>         int radix = 2;
>         BigInteger interNum1 = new BigInteger(str,radix);	//743
> 
>         //我们通常不写，则是默认成10进制转换，如下：
>         BigInteger interNum2 = new BigInteger(str);			//1011100111
>     }
> 
> 
> 
> 
>     /**
>      * 大数字的基本运算
>      * 基本运算:add(),subtract(),multiply(),divide(),mod(),remainder(),pow(),abs(),negate()
>      */
>     public void testBasic() {
>         BigInteger a = new BigInteger("13");
>         BigInteger b = new BigInteger("4");
>         int n = 3;
> 
>         //1.加
>         BigInteger bigNum1 = a.add(b);			//17
>         //2.减
>         BigInteger bigNum2 = a.subtract(b);		//9
>         //3.乘
>         BigInteger bigNum3 = a.multiply(b);		//52
>         //4.除
>         BigInteger bigNum4 = a.divide(b);		//3
>         //5.取模(需 b > 0，否则出现异常：ArithmeticException("BigInteger: modulus not positive"))
>         BigInteger bigNum5 = a.mod(b);			//1
>         //6.求余
>         BigInteger bigNum6 = a.remainder(b);	//1
>         //7.平方(需 n >= 0，否则出现异常：ArithmeticException("Negative exponent"))
>         BigInteger bigNum7 = a.pow(n);			//2197
>         //8.取绝对值
>         BigInteger bigNum8 = a.abs();			//13
>         //9.取相反数
>         BigInteger bigNum9 = a.negate();		//-13
>     }
> 
> 
> 
> 
>     /**
>      * 二进制运算(返回类型都为BigInteger，不常用，但有备无患)
>      */
>     public void testBinaryOperation() {
>         BigInteger a = new BigInteger("13");
>         BigInteger b = new BigInteger("2");
>         int n = 1;
> 
>         //1.与：a&b
>         BigInteger bigNum1 = a.and(b);			//0
>         //2.或：a|b
>         BigInteger bigNum2 = a.or(b);			//15
>         //3.异或：a^b
>         BigInteger bigNum3 = a.xor(b);			//15
>         //4.取反：~a
>         BigInteger bigNum4 = a.not();			//-14
>         //5.左移n位： (a << n)
>         BigInteger bigNum5 = a.shiftLeft(n);	//26
>         //6.右移n位： (a >> n)
>         BigInteger bigNum6 = a.shiftRight(n);	//6
>     }
> 
>     /**
>      * 毕竟大数字之间的大小
>      */
>     public void testCompare() {
>         BigInteger bigNum1 = new BigInteger("52");
>         BigInteger bigNum2 = new BigInteger("27");
> 
>         //1.compareTo()：返回一个int型数据（1 大于； 0 等于； -1 小于）
>         int num = bigNum1.compareTo(bigNum2);			//1
> 
>         //2.max()：直接返回大的那个数，类型为BigInteger
>         //	原理：return (compareTo(val) > 0 ? this : val);
>         BigInteger compareMax = bigNum1.max(bigNum2);	//52
> 
>         //3.min()：直接返回小的那个数，类型为BigInteger
>         //	原理：return (compareTo(val) < 0 ? this : val);
>         BigInteger compareMin = bigNum1.min(bigNum2);	//27
>     }
> }
> ```
>
> 