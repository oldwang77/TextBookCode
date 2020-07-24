[HDOJ 1015](http://acm.hdu.edu.cn/showproblem.php?pid=1015)
这看上去并不像一道DFS的题目，核心代码DFS中也没有搜索下一个结点的四个方向。下面的for循环，很像DFS向4个方向搜索。这里的len代表字符数组的长度。下一个搜索目标，就在这个字符序列中找，找至今还没有被访问过的"!visit[i]"，并且还没有找到解!judge，选完一个字符时，注意visit[i]=0，无后效性。

```
for(i=0;i<len;i++){
		if(!visit[i]&&!judge){ //该字符还没有被访问，并且我们还没有找到解
			b[depth]=a[i];
			visit[i]=1;
			dfs(depth+1);
			visit[i]=0;
		}

```


DFS:
```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
#include<queue>
#include<math.h>
using namespace std;
char a[15],b[10];
int visit[15];
int n,num,len;
bool judge=false;

bool cmp(char a,char b){
return a>b;
}
void init(){
	int i;
	len=strlen(a);
	judge=false;
	memset(visit,0,sizeof(visit));
	sort(a,a+len,cmp);
	for(i=0;i<len;i++){
		a[i]=a[i]-'A'+1;
	}
}
void lnit(){
	int i;
	for(i=0;i<5;i++)
		b[i]=b[i]+'A'-1;
}
bool check(){
	if(n==b[0]-b[1]*b[1]+pow(b[2],3)-pow(b[3],4)+pow(b[4],5)){
		judge=true;
		return true;
	}
	else
		return false;
}
void dfs(int depth){
	int i;
	//递归边界
	if(judge) return ; //感觉没什么用哎╮(╯▽╰)╭，有剪枝的作用？请指正。
	if(depth==5)	{check(); return ;}
	for(i=0;i<len;i++){
		if(!visit[i]&&!judge){ //该字符还没有被访问，并且我们还没有找到解
			b[depth]=a[i];
			visit[i]=1;
			dfs(depth+1);
			visit[i]=0;
		}
	}
}

int main(){
	while(scanf("%d %s",&n,a)&&!(n==0 && strcmp(a,"END"))){
		init();  //将字符数组中的a转换成数字
		dfs(0);
		lnit();	//把数字再转换成数字输出
		if(judge) cout<<b<<endl;
		else
			cout<<"no solution"<<endl;
	}
	return 0;
}
```