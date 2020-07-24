[HDOJ 2084](http://acm.hdu.edu.cn/showproblem.php?pid=2084)
这是一道非常经典的一道动态规划题目。
运用了顺推法，我感觉很像记忆化搜索。每一层每一个位置都存放着当前位置的最大值。
将一个问题分为很多子问题，每一次解决一个子问题（也就是一层，一个状态）。如果我们能保存已解决子问题的答案，用一个表记录下来，不管用到用不到，只要计算过，就放到表里。这就是**动态规划的基本思想。**

```
#include<iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=10010;
int a[maxn][maxn],dp[maxn][maxn];
int max(int x,int y){
	x=(x>y?x:y);
	return x;
	}
void get(int n){
	int i,j;
	for(i=1;i<=n;i++){
		for(j=1;j<=i;j++){
			if(i==1&&j==1) dp[i][j]=a[i][j]; //第一层第一个
			else if(j==1) dp[i][j]=dp[i-1][j]+a[i][j];//第2到n层第一列，第一列左上没元素
			else if(j==i) dp[i][j]=dp[i-1][j-1]+a[i][j]; //第2到n层最后一列，右上角没元素
			else dp[i][j]=max(dp[i-1][j],dp[i-1][j-1])+a[i][j];
		}
	}
}
int main(){
	int t,i,j,n;
	cin>>t;
	while(t--){
		cin>>n; //数塔n层
		for(i=1;i<=n;i++){
			for(j=1;j<=i;j++){
				cin>>a[i][j];
			}
		}
		get(n);
		int ans=0;
		for(j=1;j<=n;j++){
		ans=max(ans,dp[n][j]);
		}
		cout<<ans<<endl;
	}
	return 0;
}
```