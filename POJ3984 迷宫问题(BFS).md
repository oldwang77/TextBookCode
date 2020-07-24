这是我做的第一道BFS题目，也是刚刚了解什么是BFS。
以下是一些小收获：
 a.BFS层次遍历的实现依赖于队列。正是借助队列实现了层次搜索。
下面先给出BFS的核心代码：
b.通过方向数组，实现坐标的移动。
c.输出时，递归逆向输出结果。

先给出BFS实现的核心代码：
```
bool BFS(Node& Vs ,Node& Vd){
	queue<Node > Q;
	Node Vn,Vw;
	int i;

	//用于标记颜色当visit[i][j]==true时，说明节点访问过
	bool visit[maxn][maxn];
	
	//四个方向
	int dir[][2]={
		{0,1},{1,0},
		{0,-1},{-1,0}
	};

	//初始状态被放入队列Q
	Q.push(Vs);
	visit[Vs.x][Vs.y]=true//结点已经访问过

		while(!Q.empty()){ //队列不空，继续搜索
		//取出队列的头Vn
			Vn=Q.front;
			Q.pop();
		
			for(i=0;i<4;i++){
				Vw=Node(Vn.x+dir[i][0]+Vn.y+dir[i][1]);  //计算相邻结点
				
				if(Vw==Vd){  //找到结点了
					//记录路径，这里没给解法
					return true;
				}
				if(isValid(Vw) && !visit[Vw.x][Vw.y]){
					//Vw是一个合法的结点，并且没有访问过
					Q.push(Vw); //加入队列Q
					Visit[Vw.x][[Vw.y]=true; //设置结点颜色
				}
			}
		}
		return false;
}

```
回到这道题目上来：
摘自：
http://blog.csdn.net/huanghanqian/article/details/51506732
> 思路：
思迷宫是一个5 × 5的二维数组，搜索起来不会很复杂，也不会超时。从左上角（0, 0）位置开始，上下左右进行搜索，可以定义一个方向数组，代表上下左右四个方向，使用方向数组，可以使一个点上下左右移动。对于数组中的每个元素用结构体来存储，**除了有x,y成员外，还要定义pre成员，用来表示从左上角到右下角的最短路径中每个元素的前一个元素的下标，即保存路径，方便后面的输出。**通过广度搜索借助队列进行操作，当中要注意
1.搜索的元素是否在迷宫内；
2.是否可行（其中1是墙壁不能走，0可以走）。我们可以把已走过的路标记为1，这样能保证路径最短（因为广度优先搜索，只会离初始点的步数一步步增多。如果之后发现遇到了已走过的点，那肯定是从之前已走过的点那边走最短，而不是从之后发现的点走最短啊），直到搜索到右下角（4, 4），问题就解决了。输出路径即可。

下面是实现代码：

```
include<iostream>  
using namespace std;  
  
#define QSize 50  
  
int a[5][5];  
//把迷宫想象成x轴，y轴  
//int dx[4]={-1,1,0,0};//x轴方向上的变化  
//int dy[4]={0,0,-1,1};//y轴方向上的变化  
int dis[4][2]={{-1,0},{1,0},{0,-1},{0,1}};//定义4个方向  
struct Node{  
    int x,y,pre;  
}queue[QSize];//设置一个50个格子的队列  
int front=0;  
int rear=0;//设置队头和队尾，头指针指向头元素,尾指针指向队尾的下一个位置  
int visit[5][5];//记录是否访问过的数组  
  
//广度优先遍历  
void bfs(int beginX,int beginY,int endX,int endY){  
    queue[0].x=beginX,queue[0].y=beginY,queue[0].pre=-1;//将初始结点[0,0]压入队列  
    rear=rear+1;  
    visit[beginX][beginY]=1;  
    while(front<rear){//如果队列不为空  
        for(int i=0;i<4;i++){//4个方向搜索可达的方向  
            int newx=queue[front].x+dis[i][0];  
            int newy=queue[front].y+dis[i][1];  
            if(newx<0||newx>5||newy<0||newy>5||a[newx][newy]==1||visit[newx][newy]==1)//是否在迷宫内，是否撞墙，是否已走过  
                continue;  
                 //进队  
            queue[rear].x=newx;  
            queue[rear].y=newy;  
            queue[rear].pre=front;  
            rear++;  
            visit[newx][newy]=1;//给走过的位置做标记  
            if(newx==endX&&newy==endY){  
                return;  
            }  
        }  
        front++;//出队  
    }  
}  
  
void print(Node now){  
    if(now.pre==-1)  
        cout<<"("<<now.x<<", "<<now.y<<")"<<endl;  
    else{  
        print(queue[now.pre]);  
        cout<<"("<<now.x<<", "<<now.y<<")"<<endl;  
    }  
}  
  
int main(){    
   //初始化迷宫  
   for(int i=0;i<5;i++){  
       for(int j=0;j<5;j++){  
            cin>>a[i][j];  
       }        
   }  
   bfs(0,0,4,4);  
   print(queue[rear-1]);  
   return 0;  
}  

```