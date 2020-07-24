UVA10305
思路分析：
有n个点，m条边，给n个顶点做拓扑排序
AC1：
```
#include<cstdio>
#include<cstring>
#define N 100
int g[N+1][N+1],u,v,n,m,stack[N+1],vis[N+1],pos=0;
void push(int x){
    stack[pos++]=x;
}
int pop(){
    return stack[--pos];
}
void dfs(int u){
    vis[u]=-1;//visting
    for(int i=1;i<=n;i++)if(g[u][i]&&!vis[i])
        dfs(i);
    push(u);vis[u]=1;//visted
} 
int main(){
    while(scanf("%d%d",&n,&m)==2&&(m||n)){//这里是m或n，与的话会wa，我离散数学要去面壁 
        while(m--){
            scanf("%d%d",&u,&v);
            g[u][v]=1;
        }
        memset(vis,0,sizeof(vis));//initialization
        for(int i=1;i<=n;i++)//topological sort
            if(!vis[i])dfs(i);
        while(pos){//print result
            printf("%d",pop());
            printf("%c",pos>0?' ':'\n');//三目运算符真的很好用
        }
    }
	return 0;
}
```

AC2：

```
#include <iostream>
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <sstream>
#include <set>
#include <map>
#include <queue>
#include <stack>
#include <cmath>
#define nmax 200
#define MEM(x) memset(x,0,sizeof(x))
using namespace std;
vector<int> v[nmax];
int indegree[nmax];
int n;
bool suc = true;
queue<int> ans;
void topsort()
{
    queue<int> q;
    while(1){
        for(int i = 1; i<=n ;++i){
            if(indegree[i] == 0){
                q.push(i);
                ans.push(i);
                indegree[i] = -1;
            }
        }
        if(q.empty()) break;
        while(!q.empty()){
            int t = q.front(); q.pop();
            for(int j = 0;j<v[t].size();++j){
                int tt = v[t][j];
                if(indegree[tt] == -1){
                    suc = false;
                    break;
                }else indegree[tt]--;
            }
            v[t].clear();
            if(!suc) break;
        }
        if(!suc) break;
    }
    if(ans.size() <n){
        suc =false;
        return;
    }
}
void output()
{
    bool isfirst = true;
    while(!ans.empty()){
        int t = ans.front(); ans.pop();
        if(isfirst){
            printf("%d",t);
            isfirst = false;
        }else
            printf(" %d",t);
    }
    printf("\n");
}
int main()
{
    //freopen("in.txt","r",stdin);
    int m;
    while(scanf("%d%d",&n,&m) ==2 && (n||m)){
        MEM(indegree);
        suc = true;
        int a,b;
        for(int i = 0; i<m; ++i){
            scanf("%d%d",&a,&b);
            indegree[b]++;
            v[a].push_back(b);
        }
        topsort();
        if(suc) output();
        else printf("failed\n");  **//拓扑排序可以检验一个有向图是否是有向无环图**
    }
    return 0;
}
```