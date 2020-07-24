Dijkstra经典代码，模板题目。
AC：
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

	for(i=1;i<=n;i++)
		dis[i]=map[s][i]; //dis[i]指源点到点i的距离
	for(i=1;i<=n;i++){
		min=inf;
		for(j=1;j<=n;j++){
			if(dis[j]<min && !vis[j]){
				pos=j;
				min=dis[j];
			}
		}
		vis[pos]=1;
		for(j=1;j<=n;j++){  //动态规划思想
			if(dis[j]>dis[pos]+map[pos][j]&&!vis[j])
				dis[j]=dis[pos]+map[pos][j];
		}
	}
}
int main()
{
	int i,j,x,y,z,sta,end;
	while(~scanf("%d%d",&n,&m))
	{ 
		if(n==0&&m==0) break;
		for(i=1;i<=n;i++)
		{
			for(j=1;j<=n;j++)
				map[i][j]=inf;
			map[i][i]=0;
		}
		for(i=0;i<m;i++)
		{
			scanf("%d%d%d",&x,&y,&z);
			if(z<map[x][y])  //防止重路
				map[x][y]=map[y][x]=z;
		}
		Dijkstra(1,n);
		//判断有没有最短路径
		printf("%d\n",dis[n]);
	}
return 0;	
}

```
别人的AC：
```
#include <iostream>
using namespace std;

const int intmax = 10000000;
const int MAX = 102;
int Map[MAX][MAX];
bool visited[MAX];
int dist[MAX];
int n, m;
void Dijsktra()
{
	int i, j;
	for (i = 1; i <= n; i++)
		dist[i] = Map[1][i];
	visited[1] = 1;
	dist[1] = 0;
	for (j = 1; j <= n; j++) {
		int x = 1, m = intmax;
		for (i = 1; i <= n; i++) //寻找最短路
			if (!visited[i] && dist[i] < m)
				m = dist[x=i];
		visited[x] = 1;
		for (i = 1; i <= n; i++)//更新
			if (!visited[i] && Map[x][i] < intmax)
				dist[i] = min(dist[i], dist[x]+Map[x][i]);
	}
}
int main ()
{
	int a, b, cost;
	int i, j;
	while (cin >> n >> m && (n+m)) {
		for (i = 1; i <= n; i++)
			for (j = 1; j <= n; j++) {
				if (i == j) Map[i][j] = 0;
				else Map[i][j] = intmax;
			}
		while (m--) {
			cin >> a >> b >> cost;
			if (cost < Map[a][b]) {//防止重路
				Map[a][b] = cost;
				Map[b][a] = cost;
			}
		}
		memset (visited, 0, sizeof(visited));
		for (i = 1; i <= n; i++)
			dist[i] = intmax;
		Dijsktra();//从1到各点的最短路径
		cout << dist[n] << endl;	
	}
	return 0;
}
```

