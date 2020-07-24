[HDOJ 1257](http://acm.hdu.edu.cn/showproblem.php?pid=1257)
分析：
1.这是我做的第一道最长上升子序列(LIS)问题。
我开始看了半天，总觉的是最长下降子列问题，太笨了啊。其实最长上升子序列的长度就是需要配备的拦截系统的个数。
假设现在来了10个导弹，导弹高度如下：
389 207 155 300 299 170 158 65 140 150.则与之对应的dp[i]（最长上升子序列的长度）的值为：
389 207 155 300 299 170 158 65 140 150
1    	1	1    2	    2      2     2     1    2    3

重点掌握ologn的算法吧————————————————

2.O(n*n)算法
> dp[i]表示以ai为末尾的最长上升子序列的长度，而以ai结尾的最长上升子序列有两种：1.只包含ai的子序列;  2.在满足j `<`i且aj< ai的以aj为结尾的上升子序列末尾，追加上ai得到的子序列。
所以有如下递推关系：
dp[i]=max{1,dp[j]+1 |j<  i且aj< ai} 

```
#include<iostream>
#include<string.h>
#include<cstdio>
using namespace std;
int a[10010],dp[10010];
int max(int a,int b){
	return a>b?a:b;
}
int main(){
	int n,i,j;
	while(cin>>n){
		for(i=0;i<n;i++){
			cin>>a[i];
		}
	int ans=0;
	for(i=0;i<n;i++){
		dp[i]=1;
		for(j=0;j<i;j++){
			if(a[j]<a[i]){
				dp[i]=max(dp[i],dp[j]+1);
			}
		}
		ans=max(ans,dp[i]);
	}
	cout<<ans<<endl;
	}
	return 0;
}
```

3.O(nlogn)算法，dp[i]=长度为i+1的上升子序列中末尾元素的最小值（不存在的话就是INF）
**这种算法中，运用STL中的lower_bound()函数很方便，关于此函数的具体用法见：[STL二分查找**](http://blog.csdn.net/zwj1452267376/article/details/47150521)

```
#include<iostream>
#include<algorithm>
#include<string.h>
#include<cstdio>
#define INF 0x3f3f3f //把INF设置为无穷大
using namespace std;
int a[10010],dp[10010];

int main(){
	int n,i,pos;
		while(cin>>n){
		for(i=0;i<n;i++){
			cin>>a[i];
			dp[i]=INF; //仔细体会这里dp[i]设为INF的作用，
			//可以保证每一次放进一个元素时，由于这个位置是INF，
			//则要么取代前一个元素，要么在尾部放入一个元素（此时pos的位置是当前最后一个元素的下一个元素）
		}
		for(i=0;i<n;i++){
			//函数lower_bound()在first和last中的前闭后开区间进行二分查找，
			//返回大于或等于val的第一个元素位置。
			//如果所有元素都小于val，则返回last的位置
			//pos=low_bound(dp,dp+n,a[i])-n，就是把a[i]放到dp数组中，
			//pos的位置是a[i]大于等于dp的一个元素位置
		//	*low_bound(dp,dp+n,a[i])=a[i];
			pos=lower_bound(dp,dp+n,a[i])-dp;
			dp[pos]=a[i];
		}
			cout<<(lower_bound(dp,dp+n,INF)-dp)<<endl;		
	}
	return 0;
} 
```