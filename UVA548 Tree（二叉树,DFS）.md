[UVA548](https://vjudge.net/problem/UVA-548)

题意分析
给出一棵树的中序遍历和后序遍历，从所有叶子节点中找到一个使得其到根节点的权值最小。若有多个，输出叶子节点本身权值小的那个节点。 
先递归建树，然后DFS求解。

```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <string>
#include <sstream>
#define maxn 100010
#define INF 0x3f3f3f3f
using namespace std;
int in_order[maxn],post_order[maxn];
int lch[maxn],rch[maxn];
int n;
bool read_input(int *a){
	string s;
	if(!getline(cin,s)) return false;

	stringstream ss(s);
	n=0;
	int x;
	while(ss>>x) a[n++]=x;
	return n>0;
}
int build(int L1,int R1,int L2,int R2){
	if(L1>R1) return 0; //空树
	int root=post_order[R2];  //后续确定当前分支的根结点
	int p=L1;
	while(in_order[p]!=root) p++;//再在中序中找到根的位置，则中序中根左的为左子树的中序排列，根右的为右子树的中序
	int cnt=p-L1; //左子树结点的个数
	lch[root]=build(L1,p-1,L2,L2+cnt-1);//设此时左子树的长度为len，则当前的后序的前len个数据是左子树的后序排列。同理进行递归即可。
	rch[root]=build(p+1,R1,L2+cnt,R2-1);
	return root;
}

int best,best_sum; //目前为止的最优解和对应的权和

void dfs(int u,int sum){ //u是树根
	sum+=u;
	if(!lch[u]&&!rch[u]){
		if(sum<best_sum||(sum==best_sum&&u<best)){//如果路径的和相等，就选叶节点较小的哪一个
			best=u;
			best_sum=sum;
		}
	}
	if(lch[u]) dfs(lch[u],sum);
	if(rch[u]) dfs(rch[u],sum);
}


int main(){
	while(read_input(in_order)){
		read_input(post_order);
		build(0,n-1,0,n-1);
		best_sum=INF;
		dfs(post_order[n-1],0);
		cout<<best<<endl;
	}
	return 0;
}
```