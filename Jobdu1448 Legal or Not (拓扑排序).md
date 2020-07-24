Jobdu1448 Legal or Not (拓扑排序)
题目描述： 
ACM-DIY is a large QQ group where many excellent acmers get together. It is so harmonious that just like a big family. Every day,many "holy cows" like HH, hh, AC, ZT, lcc, BF, Qinz and so on chat on-line to exchange their ideas. When someone has questions, many warm-hearted cows like Lost will come to help. Then the one being helped will call Lost "master", and Lost will have a nice "prentice". By and by, there are many pairs of "master and prentice". But then problem occurs: there are too many masters and too many prentices, how can we know whether it is legal or not?We all know a master can have many prentices and a prentice may have a lot of masters too, it's legal. Nevertheless，some cows are not so honest, they hold illegal relationship. Take HH and 3xian for instant, HH is 3xian's master and, at the same time, 3xian is HH's master,which is quite illegal! To avoid this,please help us to judge whether their relationship is legal or not. Please note that the "master and prentice" relation is transitive. It means that if A is B's master ans B is C's master, then A is C's master. 
输入： 
The input consists of several test cases. For each case, the first line contains two integers, N (members to be tested) and M (relationships to be tested)(2 <= N, M <= 100). Then M lines follow, each contains a pair of (x, y) which means x is y's master and y is x's prentice. The input is terminated by N = 0.TO MAKE IT SIMPLE, we give every one a number (0, 1, 2,..., N-1). We use their numbers instead of their names.
输出： 
For each test case, print in one line the judgement of the messy relationship.If it is legal, output "YES", otherwise "NO".
样例输入： 
3 2
0 1
1 2
2 2
0 1
1 0
0 0
样例输出： 
YES
NO

**很明显这是一道判断是不是能够成有向无环图（DAG），自然地想到拓扑排序。**

```
#include<iostream>
#include<cstdio>
#include<vector>
#include<queue>
using namespace std;
vector<int > edge[501]; //邻接链表，保存与其相邻的结点
queue<int > Q; //保存入度为0的队列
int main(){
	int in_degree[500]; //统计每个结点的入度
	int n,m,i;
	while(scanf("%d%d",&n,&m)!=EOF){
		if(n==0&&m==0) break;
		for(i=0;i<n;i++){
			in_degree[i]=0; //所有的入度先设为0
			edge[i].clear();
		}
		while(m--){
			int a,b;
			scanf("%d%d",&a,&b);
			in_degree[b]++;
			edge[a].push_back(b);
		}
		while(Q.empty()==false) Q.pop(); 
			//清空上一次测试遗留的数据
			//Q.empty()返回值为true代表队列空
		for(i=0;i<n;i++){
			if(in_degree[i]==0) Q.push (i);
		}
		int cnt=0;
		while(Q.empty ()==false){
			int nowP=Q.front ();
			//读出队头元素，本题不要求写出拓扑排序，所以不处理；
			//如果要求确定，则该节点紧接着放在已经确定的拓扑排序中
			Q.pop();
			cnt++;
			for(i=0;i<edge[nowP].size ();i++){
				//将该结点以及以其为弧尾的边去掉
				in_degree[edge[nowP][i]]--;//该边所指的后继结点的入度-1
				if(in_degree[edge[nowP][i]]==0){
					Q.push(edge[nowP][i]);
				}
			}
		}
		if(cnt==n) printf("YES\n");
		else printf("NO\n");
	}
	return 0;
}
```