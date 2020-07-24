这道DP题WA了。
明天再看。
```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
using namespace std;
int T,N,Mj[101]; //T个case,P是临界概论，Mj是每个case中银行的价值
double P,Pj[101]; 	//N是每个case中银行的个数,Pj是每个银行被捕的概率
int dp[110],sum=0;
double max(int x,int y){
	x=(x>y?x:y);
	return x;
}

int main(){
	memset(Mj,0,sizeof(Mj));
	memset(Pj,0,sizeof(Pj));
	memset(dp,0,sizeof(dp));
	cin>>T;
	int i,j;
	while(T--){
		cin>>P>>N;
		P=1-P;
		for(i=1;i<=N;i++){
			cin>>Mj[i]>>Pj[i];
			sum+=Mj[i];
			Pj[i]=1-Pj[i];
		}
		dp[0]=1; //dp表示概率，d[0]什么都没偷，被捕概率0，及成功逃跑为1
		for(j=1;j<=N;j++)
			for(i=sum;i>=0;--i)
				dp[i]=max(dp[i],dp[i-Mj[i]]*Pj[j]); //第i个银行偷
			for(i=sum;i>=0;i--)			//或不偷能逃跑的概率
				if(dp[i]>P)
					break;
				cout<<i<<endl;
			}
return 0;
}
```