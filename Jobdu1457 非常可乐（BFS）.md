Jobdu1457 非常可乐（BFS）

题目描述： 
大家一定觉的运动以后喝可乐是一件很惬意的事情，但是seeyou却不这么认为。因为每次当seeyou买了可乐以后，阿牛就要求和seeyou一起分享这一瓶可乐，而且一定要喝的和seeyou一样多。但seeyou的手中只有两个杯子，它们的容量分别是N 毫升和M 毫升 可乐的体积为S （S<101）毫升(正好装满一瓶) ，它们三个之间可以相互倒可乐 (都是没有刻度的，且 S==N+M，101＞S＞0，N＞0，M＞0) 。聪明的ACMER你们说他们能平分吗？如果能请输出倒可乐的最少的次数，如果不能输出"NO"。
输入： 
三个整数 : S 可乐的体积 , N 和 M是两个杯子的容量，以"0 0 0"结束。
输出： 
如果能平分的话请输出最少要倒的次数，否则输出"NO"。
样例输入： 
7 4 3
4 1 3
0 0 0
样例输出： 
NO
3
```
#include<iostream>
#include<queue>
using namespace std;
struct N{
	int a,b,c;  //每个杯子中的可乐
	int t; //倾倒次数
};
queue<N> Q;
bool mark[101][101][101];
//对体积数组（x,y,z）标记，第一次到达体积数组(x,y,z)为有效状态，
//其余的舍去

void A_to_B(int &a,int sa,int &b,int sb){
/*倾倒函数，由容积为sa的杯子倒入容积为sb的杯子，
a,b保存原始杯中可乐的体积*/
	if(sb-b >a){ //a可以倒入b
		b+=a;
		a=0;
	}
	else{
		a-=(sb-b);
		b=sb;
	}
}
int BFS(int s,int n,int m){
	while(Q.empty()==false){
		N now=Q.front();
		Q.pop();
		int a,b,c; //abc保留三个杯中可乐的体积
		a=now.a;b=now.b;c=now.c;
		A_to_B(a,s,b,n);  //a倒入b
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}
		a=now.a;b=now.b;c=now.c;
		A_to_B(b,n,a,s);  //b倒入a
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}
		
		a=now.a;b=now.b;c=now.c;
		A_to_B(a,s,c,m);  //a倒入c
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}

		a=now.a;b=now.b;c=now.c;
		A_to_B(c,m,a,s);  //c倒入a
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}

		a=now.a;b=now.b;c=now.c;
		A_to_B(b,n,c,m);  //b倒入c
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}

		a=now.a;b=now.b;c=now.c;
		A_to_B(c,m,b,n);  //c倒入b
		if(mark[a][b][c]==false){
			mark[a][b][c]=true;
			N tmp;
			tmp.a=a;tmp.b=b;tmp.c=c;
			tmp.t=now.t+1;
			if(a==s/2 && b==s/2) return tmp.t;
			if(b==s/2 && c==s/2) return tmp.t;
			if(a==s/2 && c==s/2) return tmp.t;

			Q.push(tmp);
		}
	}
	return -1;
}

int main(){
	int i,j,k,s,n,m;
	while(cin>>s>>n>>m){
		if(s==0) break;
		if(s%2==1){  //奇数不能平分
			puts("NO");
			continue;
		}
		for(i=0;i<=s;i++){
			for(j=0;j<=n;j++){
				for(k=0;k<=m;k++){
					mark[i][j][k]=false;
				}
			}
		} 
		N tmp;
		tmp.a=s;
		tmp.b=0;
		tmp.c=0;
		tmp.t=0;
		while(Q.empty()==false)	Q.pop();
		Q.push(tmp);  //初始状态
		mark[s][0][0]=true;
		int rec=BFS(s,n,m);
		if(rec==-1) puts("NO\n");
		else cout<<rec<<endl;
	}
	return 0;
}
```