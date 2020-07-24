一.Dijkstra算法
------------
[如何理解Dijkstra算法](http://blog.csdn.net/liulizhi1996/article/details/50805346)

**Dijkstrashi适用于权值非负的情况。**
1.定义概览
------
Dijkstra(迪杰斯特拉)算法是典型的**单源最短路径算法**，**用于计算一个节点到其他所有节点的最短路径。**主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。Dijkstra算法是很有代表性的最短路径算法，在很多专业课程中都作为基本内容有详细的介绍，如数据结构，图论，运筹学等等。注意该算法要求图中不存在负权边。
问题描述：在无向图 G=(V,E) 中，假设每条边 E[i] 的长度为 w[i]，找到由顶点 V0 到其余各点的最短路径。（单源最短路径）
 

2.算法描述
------

1)算法思想：设G=(V,E)是一个带权有向图，把图中顶点集合V分成两组，第一组为已求出最短路径的顶点集合（用S表示，初始时S中只有一个源点，以后每求得一条最短路径 , 就将加入到集合S中，直到全部顶点都加入到S中，算法就结束了），第二组为其余未确定最短路径的顶点集合（用U表示），按最短路径长度的递增次序依次把第二组的顶点加入S中。在加入的过程中，总保持从源点v到S中各顶点的最短路径长度不大于从源点v到U中任何顶点的最短路径长度。此外，每个顶点对应一个距离，S中的顶点的距离就是从v到此顶点的最短路径长度，U中的顶点的距离，是从v到此顶点只包括S中的顶点为中间顶点的当前最短路径长度。
2)算法步骤：
a.初始时，S只包含源点，即S＝{v}，v的距离为0。U包含除v外的其他顶点，即:U={其余顶点}，若v与U中顶点u有边，则<u,v>正常有权值，若u不是v的出边邻接点，则<u,v>权值为∞。
b.从U中选取一个距离v最小的顶点k，把k，加入S中（该选定的距离就是v到k的最短路径长度）。
c.以k为新考虑的中间点，修改U中各顶点的距离；若从源点v到顶点u的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，修改后的距离值的顶点k的距离加上边上的权。
d.重复步骤b和c直到所有顶点都包含在S中。

3.其他
----
Dijkstra完全类似于prim算法生成最小生成树。

4.代码实现
------

```
int dijkstra(int n)
{
    //初始化v[0]到v[i]的距离
    for(int i=1;i<=n;i++)
        dis[i] = w[0][i];                                       
    vis[0]=1;//标记v[0]点
    for(int i = 1; i <= n; i++)
    {
        //查找最近点
        int min = INF,k = 0;
        for(int j = 0; j <= n; j++)
            if(!vis[w] && dis[j] < min)
                min = dis[w],k = j;
        vis[k] = 1;//标记查找到的最近点
        //判断是直接v[0]连接v[j]短，还是经过v[k]连接v[j]更短
        for(int j = 1; j <= n; j++)
            if(!vis[j] && min+w[k][j] < dis[j])
                d[j] = min+w[k][j];
    }
    return dis[j];
}
```

二.Floyed算法
----------
**floyd算法可以有负权值的边，但不能有包含负权值边组成的回路**
参考网页：http://www.cnblogs.com/rain-1/p/5033505.html
Floyed算法(实际是动态规划问题)
　　问题：权值矩阵matrix[i][j]表示i到j的距离，如果没有路径则为无穷
　　　　　求出权值矩阵中任意两点间的最短距离
　　分析：对于每一对定点u,v看是否存在一个点w使从u到w再到v的路径长度比已知路径短
　　如果有则更新从u到w的距离
　　
　　1：不用状态压缩的动态规划算法：
　　　　状态定义:d[1][i][j]表示以1作为媒介时从i到j的最短距离
　　　　　　　　 d[2][i][j]表示以1，2中的点作为媒介时从i到j的最短距离
　　　　　　　　 ……
　　　　　　　　d[n][i][j]表示以1,2， ……n中的点作为媒介时从i到j的最短距离
　　**状态转移方程d[k][i][j] = min(d[k-1][i][j], d[k-1][i][j]+d[k-1][k][j]);**
　　　　　　　　理解为把i到j的路径氛围两种情况，一种不经过k，一种经过k


　　2：状态压缩表示：
　　　　假设用d[i][j]表示从i到j的最短路径
　　　　由于d[i][j]在计算中重复使用，因此表示阶段的那一维被取消了
　　　　在没有计算出来新的d[i][j]的时候d[i][j]存储的实际是d[k-1][i][j]的值
　　　　因此可以省去表示阶段的那一维
　　　　状态转移方程：d[i][j] = min(d[i][j], d[i][j]+d[k][j]);

3.代码实现
------
没有状态压缩的floyed
```
/*  本程序中节点标号1,2，……n
    floyed算法：
        floyed算法实际上是动态规划，
        对于任意两个点u,v,看是否存在一个点w，使得从u到w再到v的距离比从u直接到v的距离短

        d[k][i][j]存储从i到j经过1, 2, ……k,的路径中的最短路径
        比如d[1][i][j]存存储经过1时从i到j的最短距离
            d[2][i][j]存储经过1, 2中的点时时从i到j的最短距离

        得到状态转移方程d[k][i][j] = min(d[k-1][i][j], d[k-1][i][k]+d[k-1][k][j]);
*/
#define M 100
#define INF 0x3f3f3f
#include <stdio.h>
#include <stdlib.h>
#include <algorithm>
using namespace std;
int d[M][M][M];
int mat[M][M];
int n, m;
void FloyedOrginal()                          
{
    //无论经不经过中间节点，任意两点间的最短距离初始化为无穷
    for(int k = 0; k <= n; k++)
        for(int i = 0; i <= n; i++)
            for(int j = 0; j <= n; j++)
                d[i][k][k] = INF;
    //不经过中间节点的时候两点间的距离初始化为两点间本身的距离
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
            d[0][i][j] = mat[i][j];
    //动态规划过程
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                d[k][i][j] = min(d[k-1][i][j], d[k-1][i][j]+d[k-1][k][j]);
}
void Init()
{
    printf("请输入节点的个数\n");
    scanf("%d", &n);
    printf("请输入已知数据的组数\n");
    scanf("%d", &m);
    for(int i = 0; i <= n; i++)
        for(int j = 0; j <= n; j++)
            mat[i][j] = INF;
    for(int i = 0; i < m; i++)
    {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        mat[u][v] = mat[v][u] = w;
    }
}
int main()
{
    Init();
    FloyedOrginal();
    printf("%d", d[n][1][n]);
    return 0;
}
```
状态压缩的Ffloyed

```
/*  本程序中节点标号1,2，……n
    floyed算法：
        floyed算法实际上是动态规划，
        对于任意两个点u,v,看是否存在一个点w，使得从u到w再到v的距离比从u直接到v的距离短

        d[i][j]存储从i点到j点的最短距离
*/
#define M 100
#define INF 0x3f3f3f
#include <stdio.h>
#include <stdlib.h>
#include <algorithm>
using namespace std;
int mat[M][M];
int n, m;
void FloyedOrginal()
{
    //用k遍历所有中间节点的可能
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                    mat[i][j] = min(mat[i][j], mat[i][k]+mat[k][j]);//是否经过k
}
void Init()
{
    printf("请输入节点的个数\n");
    scanf("%d", &n);
    printf("请输入已知数据的组数\n");
    scanf("%d", &m);
    for(int i = 0; i <= n; i++)
        for(int j = 0; j <= n; j++)
            mat[i][j] = INF;
    for(int i = 0; i < m; i++)
    {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        mat[u][v] = mat[v][u] = w;
    }
}
int main()
{
    Init();
    FloyedOrginal();
    printf("%d", mat[1][4]);
    return 0;
}
```

三.Bellman-ford算法
----------------
**Bellman_ford可以计算存在负权边的情况**
>bellman-ford算法进行n-1次更新（一次更新是指用所有节点进行一次松弛操作）来找到到所有节点的单源最短路。bellman-ford算法和dijkstra其实有点相似，该算法能够保证每更新一次都能确定一个节点的最短路，但与dijkstra不同的是，并不知道是那个节点的最短路被确定了，只是知道比上次多确定一个，这样进行n-1次更新后所有节点的最短路都确定了（源点的距离本来就是确定的）。 
现在来说明为什么每次更新都能多找到一个能确定最短路的节点：
1.将所有节点分为两类：已知最短距离的节点和剩余节点。
2.这两类节点满足这样的性质：已知最短距离的节点的最短距离值都比剩余节点的最短路值小。（这一点也和dijkstra一样）
3.有了上面两点说明，易知到剩余节点的路径一定会经过已知节点
4.而从已知节点连到剩余节点的所有边中的最小的那个边，这条边所更新后的剩余节点就一定是确定的最短距离，从而就多找到了一个能确定最短距离的节点，不用知道它到底是哪个节点。
bellman-ford的一个优势是可以用来判断是否存在负环，在不存在负环的情况下，进行了n-1次所有边的更新操作后每个节点的最短距离都确定了，再用所有边去更新一次不会改变结果。而如果存在负环，最后再更新一次会改变结果。原因是之前是假定了起点的最短距离是确定的并且是最短的，而又负环的情况下这个假设不再成立。

Bellman_ford效率太低，spfa是对它的优化。具体碰到再说。