[HDOJ 1010](http://acm.hdu.edu.cn/showproblem.php?pid=1010)
先解释一下奇偶剪枝：
>这个题目用一般的搜索无法完成，因为题目要求在指定的时间内完成，所以只好一步一步来啦，用DFS解决
但是如果这样结果会超时，网上说是用一种奇偶剪枝的方法来间断搜索时间，下面是剪枝的简单理论，一看就懂：
                          把map看作
                             0 1 0 1 0 1
                             1 0 1 0 1 0
                             0 1 0 1 0 1
                             1 0 1 0 1 0
                             0 1 0 1 0 1
                       从 0->1 需要奇数步
                       从 0->0 需要偶数步
                       那么设所在位置 (x,y) 与 目标位置 (dx,dy)
                       如果abs(x-y)+abs(dx-dy)为偶数，则说明 abs(x-y) 和 abs(dx-dy)的奇偶性相同，需要走偶数步
                       如果abs(x-y)+abs(dx-dy)为奇数，那么说明 abs(x-y) 和 abs(dx-dy)的奇偶性不同，需要走奇数步
                       **理解为 abs(si-sj)+abs(di-dj) 的奇偶性就确定了所需要的步数的奇偶性！！**
                       **而 (ti-setp)表示剩下还需要走的步数，由于题目要求要在 ti时 恰好到达，那么  (ti-step) 与 abs(x-y)+abs(dx-dy) 的奇偶性必须相同**
                  因此 **temp=ti-step-abs(dx-x)-abs(dy-y) 必然为偶数**！
                   
此题如果直接就用DFS遍历会超时，所以用如上奇偶剪枝进行优化，所谓 ”奇偶剪枝“  就是**非奇(数)同偶(数);奇偶性相同的两个数相加减结果还是偶数，反之为奇数。abs(si-sj)+abs(di-dj) 的奇偶性就确定了所需要的步数 t 的奇偶性。**                

注意有个优化条件是 图的总块数减去X的个数与t相比 ，及n*m-t<=wall，因为题目要求时间ti**恰好**到达终点，如果能跳的方块小于时间ti（跳一个方块时间是ti），那么一定不可能恰好在时间ti到达终点。
开始写这道题，参考的DFS代码不太规范，现在更新一段标准代码，以前的也留着。这题奇偶剪枝以及优化是因为要求恰好在t时刻到达，而不是在t时刻以及t时刻之内。
Present：
```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
#include<queue>
using namespace std;
int n,m,t,x1,x2,y1,y2;
int dx[]={1,0,-1,0};
int dy[]={0,1,0,-1};
char mp[10][10];
int visit[10][10];
bool judge=false;
void init(){
	int i,j;
	for(i=0;i<n;i++){
		for(j=0;j<m;j++){
			if(mp[i][j]=='S') {x1=i;y1=j;}
			else if(mp[i][j]=='D') {x2=i;y2=j;}
			else if(mp[i][j]=='X') visit[i][j]=1; //不妨将障碍直接记录为已经访问过
		}
	}
}
bool check(int x,int y){
	if(x<0||x>=n||y<0||y>=m) 
		return false;
	return true;
}
void dfs(int x,int y,int s){  //s记录时间
	int i;
	if(check(x,y)==false ) return ;
	visit[x][y]=1; // 标记已经访问过
	if(x==x2&&y==y2&&s==t){
		judge =true;
		return ;
	}
	//奇偶剪枝
	//int temp = t-((abs(x1-x2) + abs(y1-y2)));
    //if(temp<0 ||temp %2 ==1) return;
	if(((t-s)%2!=(abs(x-x2)+abs(y-y2))%2)) return ;
	for(i=0;i<4;i++){
		int newx=x+dx[i];
		int newy=y+dy[i];
		if(!visit[newx][newy]&&!judge){
			dfs(newx,newy,s+1);
			visit[newx][newy]=0;
		}
	}
}

int main(){
	int i;
	while(cin>>n>>m>>t){
		memset(mp,0,sizeof(mp));
		memset(visit,0,sizeof(visit));
		for(i=0;i<n;i++){
			scanf("%s",mp[i]);
		}
		init();
		judge=false;
		dfs(x1,y1,0);
		if(judge) printf("YES\n");
        else printf("NO\n");
	}
	return 0;
}

```

Past:
```
#include <iostream>
#include<cstdio>
#include<algorithm>
#include<math.h>
using namespace std;
char map[10][10];
int n,m,t,f,flag;
int sx,sy,ex,ey;
int dir[][2]={1,0,0,-1,0,1,-1,0};
void dfs(int x,int y,int b){
	if(flag||b>t) return ;
	if(x<0||y<0||x>=n||y>=m||map[x][y]=='X') return ;
	if(x==ex&&y==ey&&b==t){  //题目要求在时间ti恰好到达
		flag=1;
		return ;
	}
	int i;
	for(i=0;i<4;i++){
		map[x][y]='X'; //标记已经访问过，小技巧
		int dx=x+dir[i][0];
		int dy=y+dir[i][1];
		dfs(dx,dy,b+1);
		map[x][y]='.'; //不能影响下一次循环啊，对于下一次循环map[x][y]可是没有访问过得。这大概就是回溯法？求指教。
	}
}

int main(){
	int i,j,k;
	while(scanf("%d%d%d",&n,&m,&t)!=EOF){
		if(n==0&&m==0&&t==0) break;
		flag=0;
		int wall=0;
		for(i=0;i<n;i++){
			cin>>map[i];
		}
		for(i=0;i<n;i++){
			for(j=0;j<m;j++){
				if(map[i][j]=='S'){
					sx=i;
					sy=j;
				}
				if(map[i][j]=='D'){
					ex=i;
					ey=j;
				}
				if(map[i][j]=='X')
					wall++;
			}
		}
	//int temp = t-((abs(x1-x2) + abs(y1-y2)));
    //if(temp<0 ||temp %2 ==1) return;
	//奇偶剪枝
		if((t-(abs(ex-sx)+abs(ey-sy)))&1 ||n*m-wall<=t){ 
			cout<<"NO"<<endl;
		}
			else{
				dfs(sx,sy,0);
				if(flag)
					cout<<"YES"<<endl;
				else
					cout<<"NO"<<endl;
			}
	}
	return 0;
}
```