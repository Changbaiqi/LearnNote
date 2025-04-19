---
title: 数位DP模板
date: 2023-05-22 15:42:32
author: 长白崎
categories:
  - ["数据结构与算法","算法模板"]
tags:
  - "DP"
---



# 数位DP

---

## 说明：

> 数位DP是为了解决某些数字匹配上面的问题，其中经典的写法是套用DFS算法实现数位DP。
>
> 视屏教学：
>
> [数位DP](https://www.bilibili.com/video/BV1Fc411h76q)
> [E37 数位DP Windy数_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fa4y1H7J6/?spm_id_from=333.337.search-card.all.click&vd_source=fbdc4dced00d004504f57fb2f6de726b)
>
> 这里说一下，前导零的意思，就是这个数字的前面都是零，比如说我们要dp的数字的最数字的位数为123。但是我们在进行dfs的DP的时候会从1开始，那么一开始我们从1开始dfs的时候那么就是001。也就是前面占位的都是零，也就叫做前导零。







```java


import java.util.Arrays;
import java.util.Scanner;

/**
 * @description: 数位DP
 * 这个是一个数位DP的模板，这里的数位DP主要是利用DFS来实现的。
 * @author 长白崎
 * @date 2023/6/4 1:05
 * @version 1.0
 */
public class NumBitDP {
    public static void main(String[] args) {
        //测试数据
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();

        System.out.println(a);
    }


    public static long slove(int num){
        int bit[] = new int[13];

        int i =0;
        while(num!=0){
            bit[++i] = num%10;
            num/=10;
        }

        long dp[][] = new long[i+1][10];
        //初始化dp数组里面的值都为-1
        for(int y = 0 ; y<=i ; ++y)
            Arrays.fill(dp[i],-1);

        return dfs(dp,bit,i,0,true,true);
    }


    /**
     * 这个函数是数位DP的核心算法
     * @param dp DP的数组
     * @param bit 这个数组主要用来存每一位的数字，数字存储的位置为从最高位开始，比如说我们存123这个数字，那么在数组里面应该是[][3][2][1]
     * @param index 这个用于标记当前dfs的是对应数组的哪一个位置，dfs是从最高位先递归到最低位（个位）然后从低位逐渐递归到最高位
     * @param preNum 这个变量主要的作用是用于记录前面dfs所选的数字
     * @param limit 这个变量主要用于标记是否当前递归到的dfs是否有最高位限制，这个最高位限制与之前递归的dfs有关
     * @param zero 这个变量主要是用于记录是否含有前导零，这个前导零的意思指的是除这一位以外所有比当前dfs的这一位高的位数都是零，比如：
     *             123这个数字，我们利用数位dp的时候相当于记录1~123这个区域的数字有多少符合要求的，那么从最低位开始001、002、...、024
     *             这里的0代表的就是前导零
     * @return 返回当前位所能取到的合法数字的全部数量
     */
    public static long dfs(long dp[][],int bit[],int index,int preNum,boolean limit ,boolean zero){
        //如果搜到了最后一位那么就直接退出就得
        if(index==0)
            return 1;

        //如果不是前导零，并且没有最高位限制，而且有当前位数index为基准的前导数为preNum的搜索状态值那么就直接返回
        if(!zero && !limit && dp[index][preNum]!=-1)
            return dp[index][preNum];

        int maxNum = limit?bit[index]:9;
        long ans = 0;
        for(int i = 0 ; i <=maxNum ; ++i){
            //这里开始添加过滤条件。。。。。


            //这里开始正常的搜索
            if(zero && i==0)
                ans +=dfs(dp,bit,index-1,i,limit && i==bit[index] ? true:false,true);
            else
                ans+=dfs(dp,bit,index-1,i,limit && i==bit[index] ? true:false,false);

        }

        if(!zero && !limit)
            dp[index][preNum] = ans;

        return ans;
    }
}

```

