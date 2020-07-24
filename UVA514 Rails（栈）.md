[UVA514](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=7&page=show_problem&problem=455)
题目分析：火车进站，在中转站C处符合后进先出的原则，因此是一个栈。
在任意时刻，只有两种情况，要么火车从A到C，要么从C到B。
如果C中非空，则判断栈顶元素和目标元素是否相等。相等，C中出栈，不相等，C进栈，并继续比较。
如果C中空，就继续进栈。

```
#include<iostream>
#include<stack>
#define maxn 10010
using namespace std;
int n,target[maxn];


int main(){
	while(scanf("%d",&n)==1){
		stack<int > s;
		int A=1,B=1,i;
		for(i=1;i<=n;i++){
			cin>>target[i];
		}
		int ok=1;
		while(B<=n){
			if(!s.empty()&&s.top()==target[B]){
				s.pop();
				B++;
			}
			else if(A<=n) 
				s.push(A++);
			else{
				ok=0; break;
			}
		}
		printf("%s\n",ok?"Yes":"No");
	}
	return 0;
}
```