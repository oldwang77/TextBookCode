[POJ 1611](http://poj.org/problem?id=1611)
第一道并查集方面的题目。
题意分析：
>对于每组测试数据： 
这个学校有n个人，m小组，依次给出m个小组的信息，每行一个小组。 
每个小组的信息包括，k，表示有k个人，k个数字，分别是这k个人的ID。 
现在ID为0的人感染了病毒，他及他的小组成员全部会感染病毒，每个小组成员都会感染自己所在小组的成员(每个成员可以加入多个小组)。问现在总共有多少个人感染了病毒？ 
比较裸的并查集。 
我们可以把每个人看作是一个集合，对于一个小组的成员，就是不停地在做并集操作。并集的时候，同时要合并他们下面的节点个数。最后只要找到0的根是谁，输出其下的节点个数即可。

```
#include<stdio.h>
#include<iostream>
using namespace std;

const int maxn=30010; //结点数目上线
int pa[maxn]; //p[x]代表x的父节点
int rank[maxn]; //rank[x]是x的高度的上界
int num[maxn]; //num[]储存该集合中元素的个数，
				//并在集合合并时更新num[]即可
void make_set(int x){
//创建一个单元素集
	pa[x]=x;
	rank[x]=0;
	num[x]=1;
}

int find_set(int x){   //返回x所在的集合代表（及元素根结点的下标）
//带路径的压缩的查找
	if(x!=pa[x])
		pa[x]=find_set(pa[x]);
	return pa[x];
}

void union_set(int x,int y){//按秩合并x,y所在的集合
	x=find_set(x);
	y=find_set(y);
	if(x==y) return ;
	if(rank[x]>rank[y]){ //取rank较高的作为父节点
		pa[y]=x;
		num[x]+=num[y];
	}
	else{
		pa[x]=y;
		if(rank[x]==rank[y])
			rank[y]++;
		num[y]+=num[x];
	}
}

int main(){
	int n,m,x,y,i,t,j;
	while(cin>>n>>m){ //n个学生，m个group
		if(m==n&&n==0) break;
		if(m==0){
			cout<<"1"<<endl;
			continue;
		}
		for(i=0;i<n;i++)
			make_set(i);
		for(i=0;i<m;i++){
			cin>>t>>x;  //t每一组有t个人，x是序号
			for(j=1;j<t;j++){
				cin>>y;
				union_set(x,y);
				x=y;
			}
		}
		x=find_set(0); //找到0所在的树的树根（0是感染源）
		 //for(i = 0; i < n; i++)
         //    if(pa[i] == x)
         //        ans++;
        //cout << ans << endl;
		cout<<num[x]<<endl;
	}
	return 0;
}

```