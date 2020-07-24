[HDOJ 1241](http://acm.hdu.edu.cn/showproblem.php?pid=1241)
实际上是求**连通块**的题目。
DFS的搜索由原来的四个方向变为八个方向。和之前选数字或者给定出口和入口的题目不一样，本题需要全部遍历地图，当一个格子是@时，cnt++,并且搜索和它八个方向联通的所有格子，标记已经访问。visit[][]=1.如此访问结束
>给出地图规模n * m，地图中 *（星号）代表空白， @ 代表油田。一群@联通在一起称为油田块（此处的联通为八方向联通）。求地图中油田块的个数。
分析： 
既然是求解油田块的个数，自然先想到的办法就是先处理一个油田块，然后处理下一个油田块……然后依次计数油田块的个数，也就是每次处理一个油田块的时候+1。我们按照这种方法来实现。 
**与之前的选数字，或者是给出指定入口求解是否能走地图的题目不同。本题需要全部遍历地图**，也就是说需要一个一个格子来遍历地图，采用双重的for循环来实现。试想一下：当某一个格子是@时候，我们就从这个格子开始进行dfs，dfs的目的是处理掉与@相连的所有的@，于此同时计数+1。处理完成后，找到下一个是@的格子，再处理掉与此相连的@，计数+1。如此往复，直到处理完整个地图，搜索结束。 
那么不难看出，递归边界就是：这个格子在地图外边。进行递归的条件是：当且仅当这个格子是@并且还没有访问过。 
还有一点别忘记，此题判定@@相邻的条件是八向联通 也就是左上左下右上右下相邻也算联通，所以此题是八向搜素。 
```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int n, m,visit[105][105],cnt;
char mp[105][105];
int spx[] = {0,1,0,-1,1,1,-1,-1};
int spy[] = {1,0,-1,0,-1,1,1,-1};
bool check(int x, int y)
{
    if(x<0 || x>=n || y<0 || y>=m) return false;
    else return true;
}
void dfs(int x , int y)
{
    if(!check(x,y)) return;//发生越界的时候，终止递归
    visit[x][y] = 1;
    for(int i = 0;i <8 ;++i){
        int nx = x+ spx[i];
        int ny = y +spy[i];
        if(visit[nx][ny] == 0 && mp[nx][ny] =='@')//当且仅当格子是@并且没有访问过
            dfs(nx,ny);
    }
}
int main()
{
    while(scanf("%d%d",&n,&m) && n){
        for(int i = 0; i<n;++i)
            scanf("%s",mp[i]);
        cnt = 0;
        memset(visit,0,sizeof(visit));
        for( i = 0;i <n; ++i){//采用双重for循环遍历整个地图
            for(int j =0; j<m; ++j){
                if(!visit[i][j]&&mp[i][j] == '@'){//当且仅当格子是@并且没有访问过
                    cnt++;
                    dfs(i,j);
                }
            }
        }
        printf("%d\n",cnt);
    }
    return 0;
}
```