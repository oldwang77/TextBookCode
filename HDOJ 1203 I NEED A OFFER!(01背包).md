[HDOJ 1203](http://acm.hdu.edu.cn/showproblem.php?pid=1203)
这是一道典型性的动态规划01背包问题。（不是很懂哎，刚刚看了报告才知道01背包，渣渣一枚）

先不看这道题，先看一下**典型的01背包模型。**

> 例1：一个旅行者有个最多能装m公斤的背包，现在有n件物品。它们的重量分别为w1,w2,w3,.....wn。它们的价值分别为c1,c2,c3,.....cn.求背包能装的最大值。
> 输入：                                                  输出：
> 10 （最大容量） 4 （物品数）              12
> 2   1
> 3   3
> 4   5
> 7   9    

解：01背包问题。每种物品可以选择放或者不放。
设f[v]表示重量不超过V公斤的最大价值。
则状态方程：
f[v]=max{f[v],f[v-w[i]]+c[i]}  当v>=w[i]且 1<=i<=n时

```
#include<iostream>
#include<string.h>
using namespace std;
const int maxn=10010;
int w[maxn],c[maxn],f[maxn];
int i,j,m,n;
int main(){
	cin>>m>>n; //m背包容量，n件物品
	int i;
	for(i=1;i<=n;i++){
		cin>>w[i]>>c[i];  //每个物品的重量，价值
	}
	for(i=1;i<=n;i++){
		for(j=m;j>=w[i];j--){
			if(f[j]<f[j-w[i]]+c[i])
				f[j]=f[j-w[i]]+c[i];
		}
	}            //f[v]表示重量不超过V的最大价值，此时V=M.
	cout<<f[m]<<endl; //f[m]为最优解
	return 0;
}
```
下面来看这道题
>题意：Speakless现在有n万美元，他想申请出国，每个学校都有不同的申请费用a，Speakless得到这个学校offer的可能性b。求Speakless至少收到一份offer的可能性最大是多少。
思路：至少收到一份offer的可能性最大是多少不好求，可以转化为求一分都收不到的可能性最小是多少。这样就可以用01背包来解决了~
注意：**概率是相乘而不是相加**；
            因为求最小值，所以需要将数组f的初值置为1（因为概率最大就是1，一个都不选的时候收不到的概率也是1）。

```
#include<iostream>
#include<string.h>
using namespace std;
const int maxn=10010;
double f[maxn];

int main(){
	int m,n,w[maxn];
	double v[maxn];
	while((scanf("%d %d",&n,&m))&&(n||m)){
	int i,j;
	for(i=1;i<=m;i++){
		scanf("%d %lf",&w[i],&v[i]);
		v[i]=1-v[i]; //反过来计算一个都收不到的概率
	}
	for(i=0;i<=n;i++)
		f[i]=1; //初值设为1
	for(i=1;i<=m;i++)
		for(j=n;j>=w[i];j--)
			if(f[j]>f[j-w[i]]*v[i])  //求最小值
				f[j]=f[j-w[i]]*v[i];
			//f[j]=min(f[j],f[j-w[i]]*v[i]);
			printf("%.1lf%%\n",100*(1-f[n]));
	}
	return 0;
}
```