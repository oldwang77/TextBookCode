[HDOJ 1798](http://acm.hdu.edu.cn/showproblem.php?pid=1789)
>题意分析
给出n组数据，每组数据中有每份作业的deadline和score，如果不能按期完成，则要扣相应score，求每组数据最少扣除的score是多少。 
典型的贪心策略。 
既然是要求最少的扣分，那么肯定是要先完成分数最多的。所以可以推出按照分数排序。那么最佳策略应该是在deadline当天完成作业，如果那天已经占用，只能在deadline-1天完成，如果那天也被占用了，就只能在deadline-2天完成……直到推到第1天，如果还被占用的话，那么这个作业肯定是完不成了，扣除相应分数。遍历完数据，判断有哪些作业没有完成，输出其分数之和即可。
```
//先按照扣分从大到小排序，分数相同则按照截止日期从小到大排序。
//然后按顺序，从截止日期开始往前找没有占用掉的时间。
//如果找不到了，则加到罚分里面
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
struct hw{
	int score;
	int deadline;
	bool fish;
}item[1010];
int  cmp(hw x,hw y){
	if(x.score !=y.score )
		return x.score>y.score ;
	else
		return x.deadline <y.deadline ;
}
bool judge[1010];
int main(){
	int t,n;
	cin>>t;
	while(t--){
		memset(judge,0,sizeof(judge));
		memset(item,0,sizeof(item));
		cin>>n;
		int i,j;
		for(i=0;i<n;i++){
			cin>>item[i].deadline;
		}
		for(i=0;i<n;i++){
			cin>>item[i].score;
			item[i].fish =false;
		}
		sort(item,item+n,cmp);
		
		for(i=0;i<n;i++){
			for(j=item[i].deadline;j>=1;j--){ //从截止日期开始向前找，没有被占用的时间，如果找不到，就放到加罚的时间里。找到了，就在这个时间做个标记，显示已经占用。
				if(!judge[j]){
					judge[j]=true;
					item[i].fish=true;
					break;
				}
			}
		}
		int ans=0;
		for(i=0;i<n;i++){
			if(item[i].fish ==false){
				ans+=item[i].score ;
			}
		}
		cout<<ans<<endl;;
	}
	return 0;
}
		


```

