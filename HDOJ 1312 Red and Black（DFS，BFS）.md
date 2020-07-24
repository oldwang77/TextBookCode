[HDOJ 1312](http://acm.hdu.edu.cn/showproblem.php?pid=1312)
深度优先搜索，它的思想是从一个顶点v0开始，沿着一条路一直走到底，如果发现不能到达目标解，就返回上一个结点，然后从另一个结点一直走到底。

> 对于BFS，我们假设一个节点衍生出来的相邻节点平均的个数是N个，那么当起点开始搜索的时候，队列有一个节点，当起点拿出来后，把它相邻的节点放进去，那么队列就有N个节点，当下一层的搜索中再加入元素到队列的时候，节点数达到了N2，你可以想想，一旦N是一个比较大的数的时候，这个树的层次又比较深，那这个队列就得需要很大的内存空间了。
于是**广度优先搜索的缺点**出来了：在树的层次较深&子节点数较多的情况下，消耗内存十分严重。广度优先搜索适用于节点的子节点数量不多，并且树的层次不会太深的情况。
那么深度优先就可以克服这个缺点，**因为每次搜的过程，每一层只需维护一个节点。但回过头想想，广度优先能够找到最短路径，那深度优先能否找到呢？深度优先的方法是一条路走到黑，那显然无法知道这条路是不是最短的，所以你还得继续走别的路去判断是否是最短路？**
于是**深度优先搜索的缺点也出来了：难以寻找最优解，仅仅只能寻找有解。其优点就是内存消耗小，克服了刚刚说的广度优先搜索的缺点。**

法一：DFS解法：
```
#include<iostream>
#include<cstdio> //getchar 的头文件
using namespace std;
char a[25][25];
int dir[4][2]={{-1,0},{1,0},
			{0,-1},{0,1}};
int x,y,num;
bool YES(int x0,int y0) //判断该点是否符合题意
{
	if(x0<y&&x0>=0&&y0>=0&&y0<x) //x是列,y是行
		return true;
	else
		return false;
}
void DFS(int x0,int y0){
	a[x0][y0]='#';  //已经搜索的地方可以用'#'标记
	num++;  
	int i;
	for(i=0;i<4;i++){
		int xx=x0+dir[i][0];
		int yy=y0+dir[i][1];
		if(YES(xx,yy)&&a[xx][yy]=='.')
			DFS(xx,yy);
	}
}

int main(){
	int i,j,di,dj;
	while(scanf("%d%d",&x,&y)!=EOF){
		getchar();
		if(x==0&&y==0) break;
		for(i=0;i<y;i++){
			for(j=0;j<x;j++){
				scanf("%c",&a[i][j]);
				if(a[i][j]=='@'){
					di=i;
					dj=j;
				}
			}
			getchar();
		}
		num=0;
		DFS(di,dj);
		cout<<num<<endl;
	}
	return 0;
}
```

法二：BFS解法

```
#include<iostream>
#include<queue>
#include<cstdio>
using namespace std;
const int maxn=100;
const int Qsize=1000;
char a[maxn][maxn];
int num,x,y;
struct Node{
	int x,y;
}q[Qsize];
int dir[][2]={{-1,0},{1,0},
			{0,-1},{0,1}};

bool YES(int x0,int y0){
	if(x0<y&&x0>=0&&y0>=0&&y0<x)
		return true;
	else
		return false;
}
void BFS(int beginX,int beginY){
	queue<Node >q;
	Node start,endd;
	start.x =beginX; start.y =beginY;
	q.push (start); //将初始值压入队伍
	while(!q.empty ()){
		int i;
		start=q.front ();
		q.pop ();
		for(i=0;i<4;i++){
			endd.x=start.x +dir[i][0];
			endd.y =start.y+dir[i][1];
			if(YES(endd.x,endd.y)&&a[endd.x][endd.y]=='.'){
				a[endd.x][endd.y]='#'; //访问过的用#标记，小技巧
				num++;
				q.push (endd);
			}
		}
	}
}

int main(){
	int i,j,di,dj;
	while(cin>>x>>y&&(x!=0||y!=0)){
	for(i=0;i<y;i++){
		for(j=0;j<x;j++){
			cin>>a[i][j];
			if(a[i][j]=='@'){
				di=i;dj=j; //记录起始结点
			}
		}
	}
		num=1;
		BFS(di,dj);
		cout<<num<<endl;
	}
	return 0;
}
```