[HDOJ1233](http://acm.hdu.edu.cn/showproblem.php?pid=1233)
题意分析
首先给出n，代表村庄的个数
然后出n*(n-1)/2个信息，每个信息包括村庄的起点，终点，距离，
要求求出最小生成树的权值之和。
注意村庄的编号从1开始即可
直接跑prim
```
#include<iostream>
#include<stdio.h>
#include<string.h>
using namespace std;
#define maxn 101
#define inf 0x3f3f3f
int map[maxn][maxn];
int d[maxn],vis[maxn];
int n,m;
//prim算法构造最小生成树
int prim(){
	int i,j,mi,v,ans=0;
	for(i=1;i<=n;i++){
		d[i]=map[1][i]; //d[i]表示源点到其他点的距离（这里源点为点1）
		vis[i]=0;
	}
	vis[1]=1;
	for(i=1;i<n;i++){  //将集合内的顶点与集合外的顶点所构成的所有边中选取权值最小的一条边
	                	//作为生成树的边,并将集合外的那个顶点加入到集合中
		mi=inf;
		for(j=1;j<=n;j++)
			if(!vis[j]&&d[j]<mi){
				mi=d[j];
				v=j;
			}
		vis[v]=1;
		ans+=d[v];
		/*用集合内的顶点与集合外的顶点构成的边中找最小的边,
		并将相应的顶点加入集合中
		如此下去直到全部顶点都加入到集合中,即得最小生成树.*/
		for(j=1;j<=n;j++)
			if(!vis[j]&&d[j]>map[v][j])
				d[j]=map[v][j];
	}
	return ans;
}

int main(){
	int i,p1,p2,t;
	while(cin>>n&&n){
		memset(map,inf,sizeof(map));
		int m=n*(n-1)/2;
		for(i=1;i<=m;i++){
			scanf("%d%d%d",&p1,&p2,&t);
			map[p1][p2]=map[p2][p1]=t;
		}			
		cout<<prim()<<endl;
	}
	return 0;
}
```