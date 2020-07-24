[HDOJ 1342](http://acm.hdu.edu.cn/showproblem.php?pid=1342)
很经典的DFS框架。题意是输出6个满足升序的所有情况。
DFS的两个参数，一个保存当前的搜索位置，一个保存储存的个数。
当然，DFS就一个参数也可以，也不必在main函数中必须排序。只要在DFS中进行一个大小比较即可。见第二段代码。（闲的没事）
1.先排序
```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
#include<queue>
using namespace std;
int k,vis[110],a[110],b[110],t=0;
void DFS(int ans,int num){  
	int i;
	if(num==6){
	for(i=0;i<5;i++)
		cout<<b[i];
	cout<<b[5]<<endl;
	return ;
	}

	for(i=ans;i<k;i++){
		if(vis[i]==0){
			b[num]=a[i];
			vis[i]=1;
			DFS(i+1,num+1);
			vis[i]=0;
			}
		}
	}


int main(){
	while((cin>>k)&&k>0){
		int i;
		memset(vis,0,sizeof(vis));
		if(t++) cout<<endl;
		for(i=0;i<k;i++){
			cin>>a[i];
		}
		sort(a,a+k);
		DFS(0,0);
	}
	return 0;
}
```
2.不必先排序

```
#include <iostream>
#include<cstdio>
#include<string.h>
#include<algorithm>
#include<queue>
using namespace std;
int k,vis[110],a[110],b[110],t=0;
void DFS(int num){  
	int i;
	if(num==6){
	for(i=0;i<5;i++)
		cout<<b[i];
	cout<<b[5]<<endl;
	return ;
	}

	for(i=0;i<k;i++){
		if(vis[i]==0&&a[i]>b[num-1]){ //只要a[i]大于存在b[num-1]的那个数
			b[num]=a[i];
			vis[i]=1;
			DFS(num+1);
			vis[i]=0;
			}
		}
	}


int main(){
	while((cin>>k)&&k>0){
		int i;
		memset(vis,0,sizeof(vis));
		if(t++) cout<<endl;
		for(i=0;i<k;i++){
			cin>>a[i];
		}
	//	sort(a,a+k);
		DFS(0);
	}
	return 0;
}
```