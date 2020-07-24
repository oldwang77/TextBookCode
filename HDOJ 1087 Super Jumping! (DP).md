[HDOJ 1087](http://acm.hdu.edu.cn/showproblem.php?pid=1087)
这是一道经典的DP问题。寻找最大的一条递增子序列。我们从状态转移方程和边界条件两方面考虑。
假设dp[i]是从start开始到a[i]可获得的最大分数，a[i]中存储第i个点的分数值,那么可以得到
状态转移方程：
dp[i]=max(dp[i],dp[j]+a[i])
条件：
j< I 且a[i]>=a[j]，及j表示每一个在i前面，且值不大于a[i]的点，计算完dp后，很明显就是dp中的最大值。

```
#include<iostream>
#include<string.h>  //memset的头文件
#include<algorithm>
using namespace std;
const int maxn=1005;
int dp[maxn];
int a[maxn];
int max(int x,int y)
{
	x=(x>y)?x:y;
	return x;
}
int main(){
	int n,i,j;
	while(scanf("%d",&n)!=EOF&&n){
		memset(dp,0,sizeof(dp));
		for(i=0;i<n;i++){
			cin>>a[i];
			dp[i]=a[i];
		}
		for(i=0;i<n;i++){
			for(j=0;j<i;j++){
				if(a[i]>a[j])
					dp[i]=max(dp[i],dp[j]+a[i]);
			}
		}
		int ans=0;
		for(i=0;i<n;i++){
			ans=max(ans,dp[i]);
		}
		cout<<ans<<endl;
	}
return 0;
}
```