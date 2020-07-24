题目描述： 
    In an episode of the Dick Van Dyke show, little Richie connects the freckles on his Dad's back to form a picture of the Liberty Bell. Alas, one of the freckles turns out to be a scar, so his Ripley's engagement falls through. 
    Consider Dick's back to be a plane with freckles at various (x,y) locations. Your job is to tell Richie how to connect the dots so as to minimize the amount of ink used. Richie connects the dots by drawing straight lines between pairs, possibly lifting the pen between lines. When Richie is done there must be a sequence of connected lines from any freckle to any other freckle. 
输入： 
    The first line contains 0 < n <= 100, the number of freckles on Dick's back. For each freckle, a line follows; each following line contains two real numbers indicating the (x,y) coordinates of the freckle.
输出： 
    Your program prints a single real number to two decimal places: the minimum total length of ink lines that can connect all the freckles.
样例输入： 
3
1.0 1.0
2.0 2.0
2.0 4.0
样例输出： 
3.41
来源： 
2009年北京大学计算机研究生机试真题 

分析：题目大意，平面上有若干个点，用一些线段将这些点连接起来，任意两点之间都能通过一系列点连接。求一种连接方式最小的，求长度和。
可以想到求最小生成树，Kruskal算法。

**下面一段代码实现了将平面上的点转换为图上的点，将结点间直接相邻的线段抽象为连接结点的边，且权值为长度。**
```
while(cin>>n){
		for(i=1;i<=n;i++){
			cin>>list[i].x>>list[i].y;
		}
		int size=0; //抽象的边总数
		for(i=1;i<=n;i++){
			for(j=i+1;j<=n;j++){
				edge[size].a=i;
				edge[size].b=j;
				edge[size].cost=list[i].getDistance(list[j]);
				size++;
			}
		}

```
AC：
```
#include<iostream>
#include<cstdio>
#include<math.h>
#include<algorithm>
using namespace std;
#define N 110
int Tree[N],i,j;
int find_root(int x){   
    if(Tree[x]==-1) return x;
    else{
    int temp=find_root(Tree[x]);
    Tree[x]=temp;
    return temp;
    }
}
struct Edge{
	int a,b;
	double cost;
	bool operator < (const Edge &A) const{
		return cost<A.cost;
	}
}edge[6000];

struct Point{
	double x,y;
	double getDistance(Point A){  //计算点之间的距离
		double temp=(x-A.x)*(x-A.x)+(y-A.y)*(y-A.y);
		return sqrt(temp);
	}
}list[110];
int main(){
	int n;
	while(cin>>n){
		for(i=1;i<=n;i++){
			cin>>list[i].x>>list[i].y;
		}
		int size=0; //抽象的边总数
		for(i=1;i<=n;i++){
			for(j=i+1;j<=n;j++){
				edge[size].a=i;
				edge[size].b=j;
				edge[size].cost=list[i].getDistance(list[j]);
				size++;
			}
		}
		sort(edge,edge+size);
		for(i=1;i<=n;i++){
			Tree[i]=-1;
		}
		double ans=0;
		for(i=0;i<size;i++){
			int a=find_root(edge[i].a);
			int b=find_root(edge[i].b);
			if(a!=b){
				Tree[a]=b;
				ans+=edge[i].cost;
			}
		}//最小生成树
		printf("%.2lf\n",ans);
	}
return 0;
}

```