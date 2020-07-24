Jobdu1102 畅通工程
题目描述： 
    某省调查城镇交通状况，得到现有城镇道路统计表，表中列出了每条道路直接连通的城镇。省政府“畅通工程”的目标是使全省任何两个城镇间都可以实现交通（但不一定有直接的道路相连，只要互相间接通过道路可达即可）。问最少还需要建设多少条道路？
输入： 
    测试输入包含若干测试用例。每个测试用例的第1行给出两个正整数，分别是城镇数目N ( < 1000 )和道路数目M；随后的M行对应M条道路，每行给出一对正整数，分别是该条道路直接连通的两个城镇的编号。为简单起见，城镇从1到N编号。 
    注意:两个城市之间可以有多条道路相通,也就是说
    3 3
    1 2
    1 2
    2 1
    这种输入也是合法的
    当N为0时，输入结束，该用例不被处理。
输出： 
    对每个测试用例，在1行里输出最少还需要建设的道路数目。
样例输入： 
4 2
1 3
4 3
3 3
1 2
1 3
2 3
5 2
1 2
3 5
999 0
0
样例输出： 
1
0
2
998
来源： 
2005年浙江大学计算机及软件工程研究生机试真题 

```
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
#define N 1010
int Tree[N],i;
int find_root(int x){   
    if(Tree[x]==-1) return x;
    else{
    int temp=find_root(Tree[x]);
    Tree[x]=temp;
    return temp;
	}
}
int main(){
	int n,m;
	while(scanf("%d",&n)!=EOF &&n){
		scanf("%d",&m);
		for(i=1;i<=n;i++)	Tree[i]=-1;
		//刚开始时，所有的点都是孤立的集合，本身就是一个结点

	while(m--){  //读入边的信息
		int a,b;
		cin>>a>>b;
		a=find_root(a);
		b=find_root(b);
		if(a!=b) Tree[a]=b;
	}
	int ans=0;
	for(i=1;i<=n;i++){
		if(Tree[i]==-1) ans++;//根结点的个数
	}
	cout<<ans-1<<endl; //答案及为在ans个集合间再修建ans-1条道路，
	//即可使所有的结点连通
}
return 0;
}
```