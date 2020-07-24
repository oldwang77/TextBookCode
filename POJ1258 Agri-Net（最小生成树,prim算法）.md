[POJ 1258 ](http://poj.org/problem?id=1258)
最小生成树的prim算法和kruskal算法的理解：[最小生成树的prim和kruskal](http://blog.csdn.net/weinierbian/article/details/8059129/)
>题目大意：有n个农场，已知这n个农场都互相相通，有一定的距离，现在每个农场需要装光纤，问怎么安装光纤能将所有农场都连通起来，并且要使光纤距离最小，输出安装光纤的总距离
解题思路：又是一个最小生成树，因为给出了一个二维矩阵代表他们的距离，直接算prim就行了

最小生成树的prim算法：
```
//#include <bits/stdc++.h>貌似万能头文件POJ编译ERROR
#include<iostream>
#include<stdio.h>
using namespace std;
#define maxn 101
#define inf 0x3f3f3f
int map[maxn][maxn],n;
int d[maxn],vis[maxn];
//prim算法构造最小生成树
void prim(){
	int i,j,mi,v;
	for(i=1;i<=n;i++){
		d[i]=map[1][i]; //d[i]表示源点到其他点的距离（这里源点为点1）
		vis[i]=0;
	}
	for(i=1;i<=n;i++){  //将集合内的顶点与集合外的顶点所构成的所有边中选取权值最小的一条边
	                	//作为生成树的边,并将集合外的那个顶点加入到集合中
		mi=inf;
		for(j=1;j<=n;j++)
			if(!vis[j]&&d[j]<mi){
				mi=d[j];
				v=j;
			}
		vis[v]=1;
		/*用集合内的顶点与集合外的顶点构成的边中找最小的边,
		并将相应的顶点加入集合中
		如此下去直到全部顶点都加入到集合中,即得最小生成树.*/
		for(j=1;j<=n;j++)
			if(!vis[j]&&d[j]>map[v][j])
				d[j]=map[v][j];
	}
	for(d[0]=0,i=1;i<=n;i++) d[0]+=d[i];
	printf("%d\n",d[0]);
}

int main(){
	int i,j;
	while(cin>>n){
		for(i=1;i<=n;i++)
			for(j=1;j<=n;j++)
				scanf("%d",&map[i][j]);
			prim();
	}
	return 0;
}
```

