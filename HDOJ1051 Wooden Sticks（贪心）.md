[HDOJ 1051](http://acm.hdu.edu.cn/showproblem.php?pid=1051)
贪心算法。
做这道题的时候，我想到了几天前做的导弹拦截问题，HDOJ 1257，但是没能明白为什么最少需要的导弹拦截系统恰好等于最长递增子序列的长度。。。
这道题需要的最短时间等于最长下降子序列的长度。。。
http://blog.csdn.net/zwj1452267376/article/details/49981029
还是先熟悉一下LIS的ologn的算法吧，求指教。

```
#include<iostream>
	#include <cstdio>
	#include <cstring>
	#include <algorithm>
	using namespace std;
	#define INF 0x3f3f3f
	const int maxn=5010;
	int dp[maxn];
	struct stick{
		int l;
		int w;
	//	bool fish; //标记
	}a[maxn];
	int cmp(stick x,stick y){ //不妨以l为第一顺序，
								//w为第二顺序排序
		if(x.w !=y.w)
			return x.w<y.w;
		else
			return x.l<y.l;
	}
	int main()
	{
		int T,i,n;
		cin>>T;
		while(T--){
			cin>>n;
			for(i=0;i<n;++i){
				scanf("%d%d",&a[i].l,&a[i].w);
				dp[i]=INF;
			}
			sort(a,a+n,cmp);

			for(i=n-1;i>=0;i--){ //最长下降子序列，就是反向最长递增序列							 
			int pos=lower_bound(dp,dp+n,INF)-dp;
				dp[pos]=a[i].l;
			}
			cout<<(lower_bound(dp,dp+n,INF)-dp)<<endl;	
		}
			return 0;
	}


```

—————————————————————————————————
下面说一说，不用LIS，直接写这道题的方法。
先对木棍排序的时候，两个关键字，不妨第一关键字设为L，第二关键字设为W。排好序后
这段代码是这道题的关键：这段代码实现了将所有可以划分到一组的木头进行了标记。其中`x=a[j].l; y=a[j].w;` 不断更新当前的木棍的长度和重量，从而实现了将所有一组的木棍进行了标记。
核心代码：
```
for(j=i+1;j<n;j++){
		if(!a[j].fish&& y>=a[j].w&&x>=a[j].l ){
			x=a[j].l;
			y=a[j].w;
			a[j].fish=true;
		}
	}

```

>题意分析
给出T组数据，每组数据有n对数，分别代表每个木棍的长度l和重量w。第一个木棍加工需要1min的准备准备时间，对于刚刚经加工过的木棍，如果接下来的木棍l和w均小于等于上一根木棍的l和w那么就不许要准备时间，否则的话还需要1min的准备时间。求解对于每组数据，所需要的最小的准备时间是多少。 
贪心策略。 
**对木棍进行降序排序，首要关键字是l，次要关键字是w（可以颠倒）。然后选取最顶端的木棍，向下遍历，对于当前木棍，若l和w均小于等于顶层木棍，那么标记它为已处理，并且更新此时木棍的数据，继续遍历，直到没有木棍未知。**此时判断所有的木棍是否处理完，是的话输出结果，否则的话继续选取顶层没有处理的木棍，重复遍历的操作。
```
	#include <iostream>
	#include <cstdio>
	#include <cstring>
	#include <algorithm>
	using namespace std;
	const int maxn=5010;
	struct stick{
		int l;
		int w;
		bool fish; //标记
	}a[maxn];
	int cmp(stick x,stick y){ //不妨以l为第一顺序，
								//w为第二顺序排序
		if(x.l==y.l)
			return x.w>y.w;
		else
			return x.l>y.l;
	}

	int main()
	{
		int T,i,j,n;
		while(cin>>T){
		while(T--){
			memset(&a,0,sizeof(&a)); //新学的。。。
			cin>>n;
			for(i=0;i<n;i++){
				scanf("%d%d",&a[i].l,&a[i].w);
			a[i].fish=false;
			}
			sort(a,a+n,cmp);

			int sum=0;
			for(i=0;i<n;i++){
				if(a[i].fish==true)
					continue;
				int x=a[i].l ;
				int y=a[i].w ;
				a[i].fish=true;

				for(j=i+1;j<n;j++){
					if(!a[j].fish&& y>=a[j].w&&x>=a[j].l ){
						x=a[j].l;
						y=a[j].w;
						a[j].fish=true;
					}
				}
				sum++;
			}
			cout<<sum<<endl;		
			}
		}
		return 0;
	}

```