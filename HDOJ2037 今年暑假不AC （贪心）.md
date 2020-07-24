第一道贪心题目。
题意分析
给出来n组节目的起止时间，让求出所最多能观看的完整节目个数。 
贪心策略：**按照节目的结束时间升序排序，比较下一项的开始时间是否比上一项的结束时间大**，是的话计数器+1，并且更新结束时间，否则的话继续判断下一项。直到遍历完整个节目单，输出计数器的值即可。 
注意：排好序后，默认第一个节目是看的，并且此时计数器为1。 
至于为何按照结束时间排序。
贪心最重要的是选择合适的贪心策略，才能快准AC。
```
#include <iostream>
#include <stdio.h>
#include <cstring>
#include <algorithm>
using namespace std;
struct time{
	int sta;
	int ed;
}a[110];
int cmp(time a,time b){
	return a.ed<b.ed;
}
int main(){
	int n,i;
	while(scanf("%d",&n)!=EOF&&n){
		int cnt=1,temp;  //小心，每一组数据都要将cnt更新为1
		for(i=0;i<n;i++){
			cin>>a[i].sta>>a[i].ed;
		}
		sort(a,a+n,cmp);
		temp=a[0].ed;
		for(i=0;i<n-1;i++){
			if(temp<=a[i+1].sta) {
				cnt++;
				temp=a[i+1].ed;
			}
		}
		cout<<cnt<<endl;
	}
	return 0;
}
```