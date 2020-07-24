[HDOJ 1016](http://acm.hdu.edu.cn/showproblem.php?pid=1016)
素数环问题
> 1.**什么时候进行dfs**：即递归边界。满足何种情况就不进行搜索了，或者何种情况进行一个输出，亦或是利用条件判断去掉重复的情况。 
2.**怎样进行dfs**：是二重搜索（HDOJ.1342),还是四向搜索（HDOJ.1010),还是在数组中找遍所有的元素（HDOJ.1015）。也许以后还有八向搜索，全部搜索等等方式。 
不难发现本题要求的是，两个相邻的数字和为素数，那么也就是在每次搜索的时候，都判断一下前2个数字的和是否为素数，若是的话继续进行搜索，否则终止。
需要注意的是，最后还需要判断一下，最后一个数字和第一个数字的和是否为素数，因为题目的要求是素数环嘛。否则会出现多解。
为了方便判断素数，最好在初始化的时候进行素数筛。规模在50即可（n上限是19，最大就是19+18=37）。

> 对n为1的时候进行特判。 
init函数打50规模的素数表，然后把1置为访问过。若n不为1，对深度为2进行dfs。 
每次在递归调用dfs之前，首先检查一下前边2个数的和（depth-1和depth-2）是否为素数。（因为b[0]为0，当depth为2的时候也可以直接调用check函数，不用特判）。需要注意的是，当depth为n+1的时候，check需要检查两项内容：一是刚才说的前两个数的和是否为素数，二是最后一个数和第一个数的和是否为素数。这样就能保证是素数环了。
本题还有一个坑点，就是输出格式。输出可能组合的时候注意是每个数字之间有一个空格，也就是在行末尾只有一个换行符。题目还说了在每种case之后输出个空行，也就是说不是每组数据之间（原文表述是 Print a blank line after each case. 是after，不是between）。 所以最后还是有一个空行的。

AC：
```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
#include<queue>
#include<math.h>
using namespace std;
bool visit[21],prime[51];
int b[21],n;
void init(){
	int i,j;    //打表1-50之间的素数
	for(i=2;i<=sqrt(50);i++){
		if(prime[i]==0){
			for(j=2;i*j<=50;j++){
				prime[i*j]=1;
			}
		}
	}
	prime[1]=0;	visit[1]=true; b[1]=1;
}

bool check(int depth){
	if(depth==n+1) //对于最后要判断首位数字的和是否为素数
		if(prime[b[1]+b[depth-1]]==0 
		&& prime[b[depth-2]+b[depth-1]]==0) return true;
		else return false;
		else if(prime[b[depth-2]+b[depth-1]]==0) return true; //对于不是最后的直接判断前两个即可
		else return false;
}
void print(){
	int i;
	for(i=1;i<=n;i++)
		if(i==1) cout<<b[i];
		else cout<<" "<<b[i];
	cout<<endl;
}

void dfs(int depth){
	int i;
	if(false==check(depth)) return ;
	if(depth== n+1){
		print();
		return ;
	}
	for(i=2;i<=n;i++){
		if(!visit[i]){
			visit[i]=1;
			b[depth]=i;
			dfs(depth+1);
			visit[i]=0;
		}
	}
}

int main(){
	int t=1;
	init();
	while(scanf("%d",&n)!=EOF){
		printf("Case %d:\n",t++);
		if(1==n) printf("1\n");
		else
			dfs(2); //第一位是2，故从深度2开始dfs
		cout<<endl;
	}
	return 0;
}
```
