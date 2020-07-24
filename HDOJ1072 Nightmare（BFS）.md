[HDOJ1072](http://acm.hdu.edu.cn/showproblem.php?pid=1072)
这道题可以剪枝：
剪枝说明：如果当前到达该点时间少于该点剩余时间并且步数大于该点步数，则返回.
剪枝说明：http://blog.csdn.net/iaccepted/article/details/23198623
```
#include <iostream>
#include<cstdio>
#include<algorithm>
#include<queue>
const int N=10;
using namespace std;
int map[N][N],n,m;
int dir[][2]={{0,1},{0,-1},
				{1,0},{-1,0}};
struct node{
	int x,y;  //记录坐标
	int step,time;  //记录步数和时间
}start;
void BFS(){
	queue<node> q;//队列实现
	node q1,q2;
	q.push (start);
	while(!q.empty ()){
		int i;
		q1=q.front ();
		q.pop ();
		for(i=0;i<4;i++){
		q2.x=q1.x+dir[i][0];
		q2.y=q1.y +dir[i][1];
		q2.step =q1.step+1;
		q2.time =q1.time -1;
		//判断这一步是否超过矩阵的范围
		//判断这不不是走过的（或墙）或炸弹的时间已到
		if(q2.x>=0&&q2.y>=0&&q2.x<n&&q2.y<m&&map[q2.x][q2.y]!=0&&q2.time >0){
		//说明找到答案，搜索结束
			if(map[q2.x][q2.y]==3){
				cout<<q2.step<<endl ;
				return ;
			}
			else if(map[q2.x][q2.y]==4){
				q2.time =6;  //碰到时间调整器，可以恢复时间
				map[q2.x][q2.y]=0;//标记已经走过
			}
			q.push (q2); //将这一步放进队列
		}
		}	
	}
	//队列搜完都没找到答案，说明答案不存在
	cout<<"-1"<<endl;
	return ;
}

int main(){
	int i,j,T;
	scanf("%d",&T);
	while(T--){
		cin>>n>>m;
		for(i=0;i<n;i++)
			for(j=0;j<m;j++){
				cin>>map[i][j];
				if(map[i][j]==2){
					start.x=i;
					start.y=j;
					start.step =0;
					start.time =6; //时间初始化为6
				}
			}
			BFS();
	}
	return 0;
}
```