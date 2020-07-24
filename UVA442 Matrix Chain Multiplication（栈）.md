题意：给出矩阵规格， 然后给出矩阵算式， 求每个算式的计算量。当矩阵无法相乘时 输出error
思路：1.计算量的计算方式 (A.r, A.c) * (B.r, B.c) 的计算量是 A.r * A.c * B.c（）
    2.关键： **用栈在储存矩阵。 遇到 '(' 时读掉, 遇到字母时压栈， 遇到')'时计算栈的顶部两个矩阵， 形成行矩阵压入栈中。**
    3.**用数组保存读入的矩阵， 26个英文字母 对于1~26.**
```
#include<iostream>
#include<string>
#include<stack>
#define maxn 10010
using namespace std;
struct Matrix{
	int a,b;
}m[26];

stack<Matrix> s;
int main(){
	int n,i;
	cin>>n;
	for(i=0;i<n;i++){
		string name;
		cin>>name;
		int k=name[0]-'A';
			cin>>m[k].a>>m[k].b;  //用数组来记录矩阵
	}
	string expr;
	while(cin>>expr){
		int len=expr.length ();
		bool error=false;
		int ans=0;
		for(i=0;i<len;i++){
			if(isalpha(expr[i]))	s.push (m[expr[i]-'A']);
			else if(expr[i]==')') {
				Matrix m2=s.top (); s.pop ();
				Matrix m1=s.top (); s.pop ();
				if(m1.b!=m2.a ) {error=true; break;}
				ans+=m1.a *m1.b *m2.b ;
				Matrix nm;
				nm.a=m1.a ;
				nm.b=m2.b;
				s.push (nm);
			}
		}
		if(error) printf("error\n");
		else
			printf("%d\n",ans);
	}
	return 0;
}


```