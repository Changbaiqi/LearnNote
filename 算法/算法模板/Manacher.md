# Manacher

---

## 说明：

> Manacher算法主要是用于字符串查找最长回文子串，其原理就是在原本的字符串插入其他特殊符号达到把我们所有需要所有需要匹配的回文都变成了奇回文，并且以其奇回文的中点向左右扩散计算其回文长度。

## Java代码模板：

> ```java
> 
> import java.util.Scanner;
> 
> /**
>  * @description: 马拉车(Manacher)算法模板
>  * 此算法是一种匹配子串最长回文算法，可以大大减少时间复杂度
>  * @author 长白崎
>  * @date 2023/3/27 2:20
>  * @version 1.0
>  */
> public class Manacher {
> 
>     public static void main(String[] args) {
>         //以下用于测试集，可以直接忽略掉
>         Scanner sc = new Scanner(System.in);
>         String text = sc.next();
> 
>         //开始manacher算法
>         manacher(text);
> 
>     }
> 
> 
>     /**
>      * Manacher核心算法
>      * @param text
>      */
>     public static void manacher(String text){
>         String m = ""; //用于存储Manacher处理之后的字符串
>         m+="$"; //插入头标记符号
>         m+="#"; //插入特殊符号
>         for(int i=0 ; i < text.length() ; ++i){
>             m +=text.charAt(i);//插入字符串
>             m +="#"; //插入特殊符号
>         }
>         m+="^"; //插入末尾标记符号
> 
>         int data[] = new int[m.length()];
>         int i= 1; //遍历计数用的
>         int max =0; //用于记录最大回文子串长度
>         while(i < m.length()-1){
>             int j =0; //以i点为回文中心，向左右拓展j位匹配是否相同。
>             while(m.charAt(i-j-1)==m.charAt(i+j+1)) //开始匹配，直至不是回文为止
>                 ++j;
>             data[i] = j;
>             max = Math.max(max,j); //更新最长回文子串的大小
>             ++i;
>         }
> 
> 
>         System.out.println("最长回文子串的长度为："+max);
> 
>         testPrint(m,data); //这个不用管，使用的时候记得删掉，这个只是用于输出可视化数据的，没啥用
>     }
> 
>     /**
>      * 这个函数不用管，只是用于输出数据可视化，方便大家更直观的看
>      * @param m
>      * @param data
>      */
>     public static void testPrint(String m,int data[]){
>         for(int i = 0 ; i < m.length() ; ++i){
>             System.out.print(m.charAt(i)+"    ");
>         }
>         System.out.println();
>         for(int i = 0 ; i < data.length ; ++i){
>             System.out.print(data[i]+"    ");
>         }
> 
>     }
> }
> ```