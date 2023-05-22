# 数位DP

---

## 说明：

> 数位DP是为了解决某些数字匹配上面的问题，其中经典的写法是套用DFS算法实现数位DP。
>
> 视屏教学：
>
> [数位DP](https://www.bilibili.com/video/BV1Fc411h76q)







```java
import java.util.Arrays;
import java.util.Scanner;

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

