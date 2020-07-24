最小生成树的Prim算法实现。
我想说一下我对下面代码的认识：（小白一只，大家笑笑就好）

```
//更新权值
			for(j=1;j<=n;j++)
				if(visited[j]==0&&low[j]>map[pos][j])
					low[j]=map[pos][j];
				}
```
我觉得这就是松弛的操作(relaxation),遍历没有访问的结点（visit[j]==0），用 源点到未访问点A的距离(low[j]) 和 集合内访问过的点(visit[j]==1)到未访问的点A的距离(map[pos][j])比较，进行一次松弛。如果满足条件，就更新源点到未访问点的距离。重复n-1个循环，把n-1个结点都放到集合内。
```
#include<stdio.h>
#include<string.h>
#include<iostream>
#define max 0x3f3f3f
#define N 110
using namespace std;
//创建map二维数组储存图表，low数组记录每2个点间最小权值，
//visited数组标记某点是否已访问
int map[N][N],low[N],visited[N];
int n;

int prim(){
	int i,j,pos,min,result=0;
	memset(visited,0,sizeof(visited));
	//从某点开始，分别标记和记录该点（源点）
	visited[1]=1;pos=1;
	//第一次给low数组赋值
	for(i=1;i<=n;i++){
		if(i!=pos) 
			low[i]=map[pos][i];
	}
	//再运行n-1次,将剩下的n-1个点放到集合中
	for(i=1;i<n;i++){
		//找出最小权值，并记录位置
		min=max;
		for(j=1;j<=n;j++)
			if(visited[j]==0&&min>low[j]){
				min=low[j];
				pos=j;
			}
	//最小权值相加
			result+=min;
			visited[pos]=1;
	//更新权值
			for(j=1;j<=n;j++)
				if(visited[j]==0&&low[j]>map[pos][j])
					low[j]=map[pos][j];
				}
	return result;
}

int main(){
	int i,v,j,ans;
	while(cin>>n){
	//所有权值初始化为最大
	memset(map,max,sizeof(map));
	for(i=1;i<=n;i++)
		for(j=1;j<=n;j++){
			cin>>v;
			map[i][j]=v;
		}
		ans=prim();
		cout<<ans<<endl;
	}
	return 0;
}
```