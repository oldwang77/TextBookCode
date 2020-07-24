>题意分析
1. 某两点之间可能有多条通路，在跑Dij时需要用距离最小的算。 
2. 当起点和重点相等的时候，距离为0 
3. 点的编号从0开始。

题目给定了起点和终点，要求两点之间的最短距离。很明显的**Dijkstra**算法。（**从源点到其余各顶点之间的最短路径**）

 Dijkstra算法（O^2）

```
#include<iostream>
#include<cstdlib>
#include<string.h>
#include<algorithm>
using namespace std;
#define inf 1<<30
#define maxn 300

int n,m;
int map[maxn][maxn];
int vis[maxn],dis[maxn];

void Dijkstra(int s,int e){
	int i,j,min,pos;
	memset(vis,0,sizeof(vis));
	dis[s]=0;vis[s]=1; //源点（起点）

	for(i=0;i<n;i++)
		dis[i]=map[s][i]; //dis[i]指源点到点i的距离
	for(i=1;i<n;i++){
		min=inf;
		for(j=0;j<n;j++){
			if(dis[j]<min && !vis[j]){
				pos=j;
				min=dis[j];
			}
		}
		vis[pos]=1;
		for(j=0;j<n;j++){  //动态规划思想
			if(dis[j]>dis[pos]+map[pos][j]&&!vis[j])
				dis[j]=dis[pos]+map[pos][j];
		}
	}
}
int main()
{
	int i,j,x,y,z,sta,end;
	while(~scanf("%d%d",&n,&m))//n个城镇,m条道路
	{ 
		for(i=0;i<n;i++)
		{
			for(j=0;j<n;j++)
				map[i][j]=inf;
			map[i][i]=0;
		}
		for(i=0;i<m;i++)
		{
			scanf("%d%d%%d",&x,&y,&z);
			if(z<map[x][y])
				map[x][y]=map[y][x]=z;
		}
		scanf("%d%d",&sta,&end);
		Dijkstra(sta,end);
		//判断有没有最短路径
		printf("%d\n",dis[end]=inf?-1:dis[end]);
	}
return 0;	
}

```

Dijkstra算法（nlogn）优化

```
#include <stdio.h>
#include <queue>
#include <string.h>
#include <algorithm>
using namespace std;

const int inf = 1<<30;
const int L = 1000+10;

struct Edges
{
    int x,y,w,next;
};
struct node
{
    int d;
    int u;
    node (int dd = 0,int uu = 0):d(dd),u(uu) {}
    bool operator < (const node &x) const
    {
        return u>x.u;
    }
};

priority_queue<node> Q;
Edges e[L<<2];
int head[L];
int dis[L];
int vis[L];

void AddEdge(int x,int y,int w,int k)
{
    e[k].x = x,e[k].y = y,e[k].w = w,e[k].next = head[x],head[x] = k++;
    e[k].x = y,e[k].y = x,e[k].w = w,e[k].next = head[y],head[y] = k++;
}

void init(int n,int m)
{
    int i;
    memset(e,-1,sizeof(e));
    for(i = 0; i<n; i++)
    {
        dis[i] = inf;
        vis[i] = 0;
        head[i] = -1;
    }
    for(i = 0; i<2*m; i+=2)
    {
        int x,y,w;
        scanf("%d%d%d",&x,&y,&w);
        AddEdge(x,y,w,i);
    }
}

int Dijkstra(int n,int src)
{
    node mv;
    int i,j,k,pre;
    vis[src] = 1;
    dis[src] = 0;
    Q.push(node(src,0));
    for(pre = src,i = 1; i<n; i++)
    {
        for(j = head[pre]; j!=-1; j=e[j].next)
        {
            k = e[j].y;
            if(!vis[k] && dis[pre]+e[j].w<dis[k])
            {
                dis[k] = dis[pre]+e[j].w;
                Q.push(node(e[j].y,dis[k]));
            }
        }
        while(!Q.empty()&&vis[Q.top().d]==1)
            Q.pop();
        if(Q.empty())
            break;
        mv = Q.top();
        Q.pop();
        vis[pre=mv.d] = 1;
    }
}

int main()
{
    int n,m,i,j,x,y;
    while(~scanf("%d%d",&n,&m))
    {
        init(n,m);
        scanf("%d%d",&x,&y);
        Dijkstra(n,x);
        printf("%d\n",dis[y]==inf?-1:dis[y]);
    }

    return 0;
}

```