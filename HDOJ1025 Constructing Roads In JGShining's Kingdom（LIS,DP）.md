[HDOJ 1025](http://acm.hdu.edu.cn/showproblem.php?pid=1025)
题目的意思就是POOR区要RICH区的资源。我们不妨将输入的看成两列数，第一列数是有序的1 2 3 ..第二列是无序的数2 3 1...就是表示POOR区1号地点要Rich区2号地点的资源。
由于第一列是有序的，题目就是求**第二列的最长递增子序列（LIS）**。如果第二列不是递增的，则一定会出现相交的情况。在HDOJ 1257中，我们给出了LIS的两种写法，此题显然只有Ologn写法满足题意。
至于对LIS的详细讲解：[LIS的ologn的实现](http://blog.csdn.net/shuangde800/article/details/7474903)


**我用STL的方法写了一遍，结果怎么都比原来的结果多一。。。。。求指正。**
**WA的代码：**

```
#include<cstdio>
#include<iostream>
#include<algorithm>
#define INF 0x3f3f3f
using namespace std;
int dp[30010],a[30010];
int main()
{
	int n,t=0,pos;
	while(scanf("%d",&n)){
		printf("Case %d:\n",++t);
		int x,y,i;
		for(i=0;i<n;i++){
			scanf("%d%d",&x,&y);
			a[x]=y;
			dp[i]=INF;
		}
		for(i=0;i<n;i++){
			pos=lower_bound(dp,dp+n,a[i])-dp;
			dp[pos]=a[i];
		}
		int ans=lower_bound(dp,dp+n,INF)-dp;
		printf("My king, at most %d roads can be built.\n\n",ans);
	}
	return 0;
}
```

以下是Ologn的写法：
```
#include<iostream>
#include<string.h>
#include<cstdio>
using namespace std;
int a[10010],dp[10010];
//二分查找。 注意，这个二分查找是求下界的;  (什么是下界？详情见《算法入门经典》 P145) 
   // 即返回 >= 所查找对象的第一个位置（想想为什么） 
 
   // 也可以用STL的lowe_bound二分查找求的下界 
int BS(int dt[],int t[],int left,int right,int i){
	int mid;
	while(left<right){
		mid=(left+right)/2;
		if(dt[mid]>=t[i])	right=mid;
		else left=mid+1;
	}
	return left;
}

int main(){
	int n,t=0;
	while(cin>>n){
		printf("Case %d:\n",++t);
		int x,y,i;
		for(i=1;i<=n;i++){
			cin>>x>>y;
			a[x]=y;
		}
		dp[1]=a[1];
		int len=1;
		for(i=2;i<=n;i++){
			if(a[i]>dp[len])	dp[++len]=a[i];
			else{
				int pos=BS(dp,a,1,len,i);
				dp[pos]=a[i];
			}
		}
		if(len == 1) printf("My king, at most 1 road can be built.\n\n");
        else printf("My king, at most %d roads can be built.\n\n",len);
	}
	return 0;
}
```