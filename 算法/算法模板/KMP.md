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
> public class KMP {
> 
> 
>     public static void main(String[] args) {
> 
>         //测试数据集
>         boolean cmp = kmp("laskdjllsnvasldfnalsljasldfnvnzx,va;dkfapasdfalfasjgakskslbvkkzvaslkfhasklbxxvbsafhaslkkhaskkbasdfas","dfnvnzx,va;dkfa");
>         if(cmp)
>             System.out.println("匹配成功");
>         else
>             System.out.println("没有匹配的子串");
>     }
> 
> 
>     //KMP核心算法
>     public static boolean kmp(String str,String cmp){
> 
>         int i =0; //主串指针
>         int j =0; //子串指针
>         while(i < str.length()){
> 
>             //这个是用于子串回溯的
>             while(cmp.charAt(j)!=str.charAt(i) && j>0) --j;
> 
>             //用于判断当前i，j分别指向的主串和子串的字符是否匹配，匹配则子串指针j++。
>             if(str.charAt(i)==cmp.charAt(j))
>                 ++j;
>             //判断是否匹配完了最后一个，如果j的引用指针大于等于他的字符串长度说明匹配完成了，直接可以返回说有此子串。
>             if(j==cmp.length()){
>                 return true; //匹配到子串，直接返回true;
>             }
>             ++i;
>         }
> 
>         return false;//未匹配到子串，返回false;
>     }
> }
> ```