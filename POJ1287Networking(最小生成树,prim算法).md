[POJ1287](http://poj.org/problem?id=1287)
经典的最小生成树的prim代码实现（刚刚知道最小生成树,hahahha）
这道题有个注意点，就是可能出现重遍，构造临接矩阵的时候选取权重最小的边即可。
**我不知道自己理解最小生成树的prim算法对不对，先写一写。**
第一步，选取一个点作为源点，这里选的是点1.(‘map[1][i]’就是点1到其点的距离)
```
int prim(){
	int i,j,mi,v,ans=0;
	for(i=1;i<=n;i++){
		d[i]=map[1][i]; //d[i]表示源点到其他点的距离（这里源点为点1）
		vis[i]=0;

```

第二步：遍历和源点外的其他点，找到和源点距离最近的那个点，标记该点已经访问，ans加上源点和它的距离。（应该是用集合内部的点与集合外部的点构成的最小边，并将该点加入集合，（标记vis[i]=1））
```
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

```
第三步：这一步我不是很懂。大概是更新源点到其他点的距离（如果新加入的点到其他点距离更短），然后回到第一步，循环。

```
/*用集合内的顶点与集合外的顶点构成的边中找最小的边,
		并将相应的顶点加入集合中
		如此下去直到全部顶点都加入到集合中,即得最小生成树.*/
		for(j=1;j<=n;j++)
			if(!vis[j]&&d[j]>map[v][j])
				d[j]=map[v][j];
	}

```

最小生成树的prim代码：
```
#include<iostream>
#include<stdio.h>
#include<string.h>
using namespace std;
#define min(a,b) a<b?a:b
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
		scanf("%d",&m);
		for(i=1;i<=m;i++){
			scanf("%d%d%d",&p1,&p2,&t);
			//构造临接矩阵，考虑到重边，取最小的路径
			map[p1][p2]=map[p2][p1]=min(map[p1][p2],t); 
		}
			
		cout<<prim()<<endl;
	}
	return 0;
}
```
