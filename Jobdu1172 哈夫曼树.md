>哈夫曼树，也叫最优树。给定n个结点和它们的权值，以它们为叶结点构造一棵带权路径和最小的二叉树，该二叉树就是哈夫曼树。

为方便取得权值最小的两个结点，引入STL中的优先队列。

```
priority_queue<int > Q;
```
这个默认的是最大顶堆，及堆顶取出的是最大的两个元素。我们要最小顶堆。则改为：

```
priority_queue<int ,vector<int >,greater<int >> Q
```

题目描述： 
哈夫曼树，第一行输入一个数n，表示叶结点的个数。需要用这些叶结点生成哈夫曼树，根据哈夫曼树的概念，这些结点有权值，即weight，题目需要输出所有结点的值与权值的乘积之和。
输入： 
输入有多组数据。
每组第一行输入一个数n，接着输入n个叶节点（叶节点权值不超过100，2<=n<=1000）。
输出： 
输出权值。
样例输入： 
5  
1 2 2 5 9
样例输出： 
37
来源： 
2010年北京邮电大学计算机研究生机试真题 

```
#include <iostream>
#include<queue>
#include<cstdio>
using namespace std;
/*有一个错误一定要小心，我调试40分钟才看出来，
我开始写成了priprity_queue<int ,vector<int>,greater<int>> Q,
实际上greater<int>> Q那两个>>有歧义了！！*/
priority_queue<int, vector<int>, greater<int> > Q;
int main(){
    int n,i;
    while(scanf("%d",&n)!=EOF){
        while(Q.empty()==false) Q.pop(); //清空堆栈
        for(i=1;i<=n;i++){
            int x;
            scanf("%d",&x);
            Q.push(x);
        }
        int ans=0;
        while(Q.size()>1){
            int a=Q.top();
            Q.pop();
            int b=Q.top();
            Q.pop();
            ans+=a+b;
            Q.push(a+b);
        }
        printf("%d\n",ans);
    }
    return 0;
}

```

