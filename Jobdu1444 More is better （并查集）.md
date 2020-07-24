Jobdu1444 More is better
题目：
Mr Wang wants some boys to help him with a project. Because the project is rather complex, the more boys come, the better it will be. Of course there are certain requirements.Mr Wang selected a room big enough to hold the boys. The boy who are not been chosen has to leave the room immediately. There are 10000000 boys in the room numbered from 1 to 10000000 at the very beginning. After Mr Wang's selection any two of them who are still in this room should be friends (direct or indirect), or there is only one boy left. Given all the direct friend-pairs, you should decide the best way. 
输入： 
The first line of the input contains an integer n (0 ≤ n ≤ 100 000) - the number of direct friend-pairs. The following n lines each contains a pair of numbers A and B separated by a single space that suggests A and B are direct friends. (A ≠ B, 1 ≤ A, B ≤ 10000000) 
输出： 
The output in one line contains exactly one integer equals to the maximum number of boys Mr Wang may keep. 
样例输入： 
4
1 2
3 4
5 6
1 6
4
1 2
3 4
5 6
7 8
样例输出： 
4
2

分析：并查集基本模型。我们可以表示每个集合的根结点记录该集合所包含的元素个数，合并累加被合并两个集合元素的个数。
```
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
#define N 100010
int Tree[N],i,sum[N];
int find_root(int x){   
    if(Tree[x]==-1) return x;
    else{
    int temp=find_root(Tree[x]);
    Tree[x]=temp;
    return temp;
    }
}
int main(){
    int n;
    while(scanf("%d",&n)!=EOF ){
        for(i=1;i<N;i++) {
			Tree[i]=-1;
			sum[i]=1; //所有结合的个数为1
		}

    while(n--){  //读入边的信息
        int a,b;
        cin>>a>>b;
        a=find_root(a);
        b=find_root(b);
        if(a!=b) 
		{
			Tree[a]=b;
			sum[b]+=sum[a];//合并子集时，
			//将成为子树的树的根结点保存该集合元素的个数累加合并得到新的树根
		}
    }
    int ans=1;
    for(i=1;i<=N;i++){
        if(Tree[i]==-1&&sum[i]>ans) ans=sum[i];
    }
    cout<<ans<<endl; 
}
return 0;
}
```

