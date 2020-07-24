[HDOJ 1035](http://acm.hdu.edu.cn/showproblem.php?pid=1035)
给出地图规模n * m, 给出入口坐标(0,y),遵循以下规则，求解机器人能否走出地图。若能，输出走出地图所需要的步数，若不能，输出进入循环前走的步数和循环的步数。
规则： 
若当前格子为N，则只能向上走，若为S向下走，E向右走，W向左走。
我第一感觉是模拟题，因为对于每个格子状态是唯一的，只有1组解：要么能走出去，要么不能。分别求出步数就行了，但感觉dfs能做，决定还是按照dfs的方法试一试。 
分析一波： 
递归边界就是机器人走出了地图或者是机器人走回到了走过的地方（吃回头草了），即可判定输出了。那么需要记录的东西就是当前走的步数，和循环的步数。当前走的步数好说，递归传参+1就行了，循环的步数想想也不难：当下一步就要吃回头草的时候，两个状态的步数之差就是循环的步数。与先前的双重搜索，四向搜索不同，dfs中要判断这个格子的字符是什么，然后决定如何走下一步。
>首先有3个全局变量保存着结果，分别是step,loop,beloop，分别保存着走出地图用的步数，循环的步数，在循环之前的步数。 
main函数完成初始化，check函数检查是否走出地图，若走出地图则judge置为true并且终止递归。每一步把当前的步数保存在visit[x][y]中，并且根据visit[x][y]是否为0判断是否吃了“回头草”。最后别忘了及时更新loop和beloop。
应该来说是一道简单的dfs应用题。
```
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=11;
int n,m,y,loop,step,beloop;
int visit[maxn][maxn];
char maze[maxn][maxn];
bool judge=false;
bool check(int x,int y){
	if(x<0||x>=n||y<0||y>=m){
		judge=true;
		return false;
	}
	else return true;
}

void dfs(int x,int y,int s){
	if(!check(x,y)) return ;
	step=s;
	if(!judge){
		if(!visit[x][y]){
		visit[x][y]=s;
		if(maze[x][y] == 'N') dfs(x-1,y,s+1);
			else if(maze[x][y] == 'S') dfs(x+1,y,s+1);
            else if(maze[x][y] == 'E') dfs(x,y+1,s+1);
            else if(maze[x][y] == 'W') dfs(x,y-1,s+1);
		}
		else{
		beloop=visit[x][y]-1; //beloop循环  的步数
		loop=s-visit[x][y];		//s走出地图的步数，loop循环之前的步数
		}
	}
	
}

int main(){
	//难点在于如何记录步数
	int i;
	while(cin>>n>>m&&n){
		cin>>y;y--;
		for(i=0;i<n;i++){
			scanf("%s",maze[i]);
		}
		judge=false;
		memset(visit,0,sizeof(visit));
		dfs(0,y,1);
		if(judge)  printf("%d step(s) to exit\n",step);
        else printf("%d step(s) before a loop of %d step(s)\n",beloop,loop);

    }
    return 0;
}

```