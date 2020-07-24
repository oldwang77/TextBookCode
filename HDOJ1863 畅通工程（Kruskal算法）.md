[HDOJ1863](http://acm.hdu.edu.cn/showproblem.php?pid=1863)
最小生成树的Kruskal算法。Kruskal借助了并查集的思想。
实现Kruskal算法的工作要求有三个要点：
1.选择一条权值最小的边。

```
qsort(a,n,sizeof(a[0]),cmp);

```
2.判断一条边的两端是否属于同一棵树（一旦属于同一棵树，就意味着一条边的两端有共同的根结点，也就就构成了回路）。这一步借助并查集的find_set完成。

```
int find(int x){   //返回x所在的集合代表（及元素根结点的下标）
//带路径的压缩的查找,回溯的过程中压缩路径很巧妙
    if(x!=par[x])
        par[x]=find(par[x]);
    return par[x];
}
```
3.合并两棵树，借助并查集中的Union可以完成。

```
void unite(int x,int y){
	x=find(x);
	y=find(y);
	if(Rank[x]<Rank[y]){
		par[x]=y;
	}
	else{
		par[y]=x;
		if(Rank[x]==Rank[y])  Rank[x]++;
	}
}

```

以下是Kruskal构造最小生成树的完整代码：
```
#include<iostream>
#include<cstdlib>
#include<cstdio>
#define maxn 10010
using namespace std;

int par[maxn],Rank[maxn];
typedef struct{
	int a,b,price;
}Node;
Node a[maxn];

int cmp(const void* a,const void *b){
	return ((Node*)a)->price -((Node*)b)->price;
}
void Init(int n){  //把所有的点构造成一棵树
	int i;
	for(i=0;i<n;i++){
		Rank[i]=0;
		par[i]=i;
	}
}

int find(int x){   //返回x所在的集合代表（及元素根结点的下标）
//带路径的压缩的查找,回溯的过程中压缩路径很巧妙
    if(x!=par[x])
        par[x]=find(par[x]);
    return par[x];
}

void unite(int x,int y){
	x=find(x);
	y=find(y);
	if(Rank[x]<Rank[y]){
		par[x]=y;
	}
	else{
		par[y]=x;
		if(Rank[x]==Rank[y])  Rank[x]++;
	}
}
int	Kruskal(int n,int m){
	int nEdge=0,res=0,i;
	//选择一条权值最小的边
	qsort(a,n,sizeof(a[0]),cmp);
	for(i=0;i<n&&nEdge!=m-1;i++){
		//判断当前的这两个端点是否属于同一棵树
		if(find(a[i].a)!=find(a[i].b)){
			unite(a[i].a,a[i].b);
			res+=a[i].price;
			nEdge++; //边数加一
		}
	}
	//如果加入边的数量小于m-1，则表明该无向图不连通，
	//等价于不存在生成最小生成树
	if(nEdge<m-1) res=-1;
	return res;
}
int main(){
	int n,m,ans,i;
	while(scanf("%d%d",&n,&m)&&n){
		Init(m);
		for(i=0;i<n;i++){
			scanf("%d%d%d",&a[i].a,&a[i].b,&a[i].price);
			//将村庄编号为0~m-1
			a[i].a--;
			a[i].b--;
		}
		ans=Kruskal(n,m);
		if(ans==-1) printf("?\n");  //不连通
		else printf("%d\n",ans);
	}
	return 0;
}
```