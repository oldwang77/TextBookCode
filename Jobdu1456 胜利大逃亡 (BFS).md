Jobdu1456 胜利大逃亡 (BFS)

题目描述： 
Ignatius被魔王抓走了,有一天魔王出差去了,这可是Ignatius逃亡的好机会.魔王住在一个城堡里,城堡是一个A*B*C的立方体,可以被表示成A个B*C的矩阵,刚开始Ignatius被关在(0,0,0)的位置,离开城堡的门在(A-1,B-1,C-1)的位置,现在知道魔王将在T分钟后回到城堡,Ignatius每分钟能从一个坐标走到相邻的六个坐标中的其中一个.现在给你城堡的地图,请你计算出Ignatius能否在魔王回来前离开城堡(只要走到出口就算离开城堡,如果走到出口的时候魔王刚好回来也算逃亡成功),如果可以请输出需要多少分钟才能离开,如果不能则输出-1.
输入： 
输入数据的第一行是一个正整数K,表明测试数据的数量.每组测试数据的第一行是四个正整数A,B,C和T(1<=A,B,C<=50,1<=T<=1000),它们分别代表城堡的大小和魔王回来的时间.然后是A块输入数据(先是第0块,然后是第1块,第2块......),每块输入数据有B行,每行有C个正整数,代表迷宫的布局,其中0代表路,1代表墙。
输出： 
对于每组测试数据,如果Ignatius能够在魔王回来前离开城堡,那么请输出他最少需要多少分钟,否则输出-1.
样例输入： 
1
3 3 4 20
0 1 1 1
0 0 1 1
0 1 1 1
1 1 1 1
1 0 0 1
0 1 1 1
0 0 0 0
0 1 1 0
0 1 1 0 
样例输出： 
11
>很经典的BFS搜索，利用queue实现层次遍历。
这道题不同的地方，就是是一个三维的空间，我们可以将迷宫中的点用三维坐标(x,y,z)表示。然后由一个点到下一个点，可以看成状态的扩展，利用坐标转换数组dir[][3]....

AC：
```
#include<iostream>
#include<queue>
using namespace std;
#define maxn 50
bool mark[maxn][maxn][maxn];
int maze[maxn][maxn][maxn];
struct N{
	int x,y,z;
	int t;
};
int dir[][3]={
	1,0,0,
	-1,0,0,
	0,1,0,
	0,-1,0,
	0,0,1,
	0,0,-1
};

queue<N> Q;

int BFS(int a,int b,int c){
	while(Q.empty ()==false){ //队列中有元素可以扩展
		int i;
		N now=Q.front ();
		Q.pop();
		for(i=0;i<6;i++){
			int nx=now.x+dir[i][0];
			int ny=now.y+dir[i][1];
			int nz=now.z+dir[i][2];
			if(nx<0||nx>=a||ny<0||ny>=b||nz<0||nz>=c) continue;
			if(maze[nx][ny][nz]==1) continue; //围墙
			if(mark[nx][ny][nz]==true) continue;

			N tmp;
			tmp.x =nx;tmp.y =ny;tmp.z=nz;
			tmp.t =now.t +1;
			Q.push (tmp);
			mark[nx][ny][nz]=true;
			if(nx==a-1&&ny==b-1&&nz==c-1) return tmp.t ;
		}
	}
	return -1; //所有的状态都找不到，返回-1
}

int main(){
	int T;
	cin>>T;
	while(T--){
		int a,b,c,t,i,j,k;
		cin>>a>>b>>c>>t;
		for(i=0;i<a;i++){
			for(j=0;j<b;j++){
				for(k=0;k<c;k++){
					cin>>maze[i][j][k];
					mark[i][j][k]=false;
				}
			}
		}
		while(Q.empty()==false ) Q.pop();
		mark[0][0][0]=true;
		N tmp;
		tmp.t =tmp.x =tmp.y =tmp.z =0;
		Q.push (tmp);  //初始状态
		int rec=BFS(a,b,c);
		if(rec<=t) cout<<rec<<endl;
		else cout<<"-1"<<endl;
	}
	return 0;
}
```