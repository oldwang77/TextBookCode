>题意分析
学校共有n个人，先给出m组信息，求问学校最多有多少种宗教信仰信奉者。 
每组信息包括a,b两个人，表示a，b两人信奉同一种宗教。 
初始化集合时，把每一个人都当做一个宗教。 **读入信息时，如果发现2个人信奉的宗教不同(根节点祖先不同)**，合并这两个人，并使得宗教总数目-1， 最后输答案即可。
```
#include<stdio.h>
#include<iostream>
#define MEM(x) memset(x,0,sizeof(x))
using namespace std;
const int maxn=50005;
int ans;
int rank[maxn],pa[maxn];
void make_set(int x){
	pa[x]=x;
	rank[x]=0;
}
int find_set(int x)
{
    int r  = x,temp;
    while(pa[r] != r) r = pa[r];
    while(x != r){
        temp = pa[x];
        pa[x] = r;
        x = temp;
    }
    return r;
}
/*
int find_set(int x){
	if(x!=pa[x])
		pa[x]=find_set(pa[x]);
	return pa[x];
}*/
void union_set(int x,int y){
	x=find_set(x);
	y=find_set(y);
	if(x==y) return ;
	ans--;//发现两人的根结点不同（题目中就代表信奉的宗教不同，ans--即可）
	if(rank[x]>rank[y]){
		pa[y]=x;
	}else{
		pa[x]=y;
		if(rank[x]==rank[y])
			rank[y]++;
	}
}

int main(){
	int n,t,i,kase=1;
	while(cin>>n>>t&&(n!=0||t!=0)){
		for(i=1;i<=n;i++)
			make_set(i);  //把每个点转换成一个集合
		ans=n;
		int a,b;
		for(i=0;i<t;i++){
			cin>>a>>b;
			union_set(a,b);
		}
		printf("Case %d: %d\n",kase++,ans);
	}
	return 0;
}
```