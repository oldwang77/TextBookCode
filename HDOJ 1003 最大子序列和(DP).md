[HDOJ 1003](http://acm.hdu.edu.cn/showproblem.php?pid=1003)
>DP问题基本思想：动态规划算法和分治算法类似，基本思想也是将带求解问题分为若干个子问题，先求子问题，然后从子问题的解得到原问题的解。和分治算法不同的是，适合动态规划的问题，经分解的子问题往往是互相不独立的。用分治法求解时，有些子问题被重复算了多遍。**如果我们能保存已解决子问题的答案，用一个表记录下来，不管用到用不到，只要计算过，就放到表里。这就是动态规划的基本思想。**

摘自：http://blog.csdn.net/pengwill97/article/details/55139996
题意分析
给出一段数字序列，求出最大连续子段和。典型的动态规划问题。
用数组a表示存储的数字序列，sum表示当前子段和，maxsum表示最大子段和。不妨设想：当sum为负数的时候： 
1.当下一个数字a[i]为正数的时候，sum+a[i] < a[i]，不如将sum归零重新计算 
2.当下一个数字为负数的时候，sum+a[i]< 0 ，若再下一个数字还为负数，依旧可以得出和小于零……直到遇到一个正数，此时回到1的情况，不如将sum归零计算。 
综上所述，当sum为负数的时候，归零。
那么再看sum为正数的时候： 
1.当下一个数字a[i]为正数的时候，当然选择加上a[i]，并且可以更新maxsunm； 
2.当下一个数字a[i]为负数的时候，由于不知道后面数字的情况，无法做出决策。 
综上所述，当sum>maxsum的时候，要更新maxsum，并且一直累加a[i]。
题目还要求输出这个子段的start位置和end位置。可以用x,y分别表示当前最优（大）的子段的开始和结束位置，然后再用sta和ed变量表示当前子段的开始和结束位置。结合上面的叙述： 
**1.当sum>maxsum的时候，即需要更新的时候，就要更新x和y的位置； 
2.当sum< 0的时候，即需要使sum归零计算的时候，就需要把sta的位置置为i+1(指向下一个位置的数字)；
以上分析过程就是DP的过程，不难设计出程序。**

—————————————————————————————————
我的小看法：
1.要注意，x,y存的才是取得最大值时候的头和尾，而sta和ed只是当前的一个状态。
2.只有在更新maxsum的时候，才更新x,y的值。
3.一旦sum<0.直接把sum=0,将sta的位置调节成i+1.因为无论sum后的a[I]的正负，一定从a[i]开始取数大于先前sum的sta开始取数。
4.后面两个if的判断不能颠倒，因为第二个if指的是舍弃先前的，从新计算。但是有可能所有的数都是负的，第二个if在前，就会一直舍弃，而没法更新第一个if的maxsum。
```
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#define nmax 100005
using namespace std;
int a[nmax];
int main()
{
    int t;
    scanf("%d",&t);
    for(int i = 1; i<= t; ++i){
        if(i!=1) printf("\n");
        printf("Case %d:\n",i);
        int n,maxsum = 0,sum = 0,x =1, y=1,sta = 1, ed = 1;
        scanf("%d",&n);
        for(int i = 1;i <=n; ++i) scanf("%d",&a[i]);
        maxsum = -1001;//2.将maxsum初始为-1001
        sum = 0;
        for(int i =1; i<=n; ++i){
            sum+=a[i];ed = i;
            if(sum>maxsum){//1.注意此处2个if的位置不能颠倒
                maxsum = sum;
                x = sta; y = i;
            }
            if(sum <0){
                sta =  i+1;
                sum = 0;
            }
        }
        printf("%d %d %d\n",maxsum,x,y);
    }
    return 0;
}
```
