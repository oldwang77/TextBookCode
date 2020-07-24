[HDOJ 1728](http://acm.hdu.edu.cn/showproblem.php?pid=1728)

开始拿到这道题，想到先前写过的BFS，由出口到终点，最短路径。后来一看题目不是这个意思，**题目要求拐弯数最少到达终点而不是求最短路径。**
看了解题报告，有了一点点思路，就是先选定一条方向，然后把该方向上所有能拐的弯都拐了。也就是说，运动的方向不仅仅是周围的四个点，因为没求最短时间，为了避免BFS提前结束，就把四个方向搜索到底。
注意，这里记录拐弯数的K的初始值不是0而是-1，因为第一个选定的方向不算在内。
————————————————————————————————
然而我写了半天，还是WA，先记下自己的错误代码，以后回头看。
WA：

```
#include<iostream>
#include<string.h>
#include<queue>
#include<cstdio>
using namespace std;
const int maxn=101;
char a[maxn][maxn];
struct Node{
int x,y,k; //k存放转弯次数
};
int dx[]={0,0,-1,1};
int dy[]={1,-1,0,0};  
int t,k,m,n,x1,x2,y1,y2,visit[maxn][maxn];

void BFS(Node vs,Node vd){
	int flag=0;
	queue<Node>q;
	Node vn,vw;
	vn.x=vs.x; vn.y=vs.y;
	vn.k=-1;  //k是-1,不是0.因为第一次选的一个方向不算次数
	visit[vs.x][vs.y]=1;
	q.push(vn);

	while(!q.empty ()){
		vn=q.front (); //取出队头
		q.pop();
		if(vn.x==vd.x&&vn.y ==vd.y &&vn.k<=k){  //到达终点
			flag=1; break;
		}
		
		vw.k=vn.k+1; //前面搜完了一个方向，肯定要转弯
		
		{
			int i;
			for(i=0;i<4;i++){
				int newx=vn.x +dx[i];
				int newy=vn.y +dy[i];
				
				while(newx>=0&&newx<n&&newy>=0&&newy<m&&a[newx][newy]!='*')
				{
				if(!visit[newx][newy]){
					vw.x =newx;  vw.y =newy;
					q.push(vw);  //符合，压入队列
					visit[newx][newy]=1; //标记已经访问
				}
				newx+=dx[i];  //单方向优先扩展
				newy+=dy[i];
				}
			}
		}
	}
	printf("%s\n",flag?"yes":"no");
}

int main(){
	int t,i,j;
	memset(visit,0,sizeof(visit));
	cin>>t;
	while(t--){
		Node vs,vd;
		cin>>n>>m;
		for(i=0;i<n;i++)
        scanf("%s",a[i]);
		cin>>k>>y1>>x1>>y2>>x2;
		vs.x=x1-1;  vs.y=y1-1;
		vd.x=x2-1;  vd.y=y2-1;

		BFS(vs,vd);
	}
	return 0;
}
```
——————————————————————————————--------
AC：
```
#include<stdio.h>
#include<string.h>
#include<queue>
#include<algorithm>
using namespace std;
struct node
{
    int x,y,k;//k存拐弯数
};
int t,n,m,x1,x2,y1,y2,k,vis[110][110];
char map[110][110];
int dx[]={0,0,-1,1};
int dy[]={1,-1,0,0};
void bfs(node vs,node vd)
{
    int flag=0;
    queue<node>q;
    node vn,vw;
    vn.x=vs.x;
    vn.y=vs.y;
    vn.k=-1;//-1,不是0；
    vis[vs.x][vs.y]=1;
    q.push(vn);
    while(!q.empty())
    {
        vn=q.front();//返回第一个元素
        q.pop(); //pop没有返回值，弹出第一个元素
        if(vn.x==vd.x&&vn.y==vd.y&&vn.k<=k)
        {
            flag=1;break;
        }

        vw.k=vn.k+1;//因为前面搜完了一个方向，就肯定会拐弯，所以要+1，也因此起点的k初始化为-1;

        {
            int i;
            for(i=0;i<4;i++)
            {
                int a=vn.x+dx[i];
                int b=vn.y+dy[i];
                while(a>=0&&a<n&&b>=0&&b<m&&map[a][b]!='*')//搜完一个方向
                {
                    if(!vis[a][b])
                    {
                        vis[a][b]=1;
                        vw.x=a;vw.y=b;
                        q.push(vw);
                    }
                    a+=dx[i];   //单方向优先扩展
                    b+=dy[i];
                }
            }
        }
    }
    printf("%s\n",flag?"yes":"no");
}
int main()
{
    scanf("%d",&t);
    while(t--)
    {
        node vs,vd;
        int i;
        memset(vis,0,sizeof(vis));
        scanf("%d%d",&n,&m);
        for(i=0;i<n;i++)
        scanf("%s",map[i]);
        scanf("%d%d%d%d%d",&k,&y1,&x1,&y2,&x2);//注意他说的行列和常规的想法不一样，免得自己搞混，所以我先输入的y。
        vs.x=x1-1;vs.y=y1-1;
        vd.x=x2-1;vd.y=y2-1;
        bfs(vs,vd);
    }
    return 0;
}

```