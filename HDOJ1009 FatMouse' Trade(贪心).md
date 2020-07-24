[HDOJ 1009](http://acm.hdu.edu.cn/showproblem.php?pid=1009)
题意分析：
每组数据，给出有的猫粮m与房间数n，接着有n行，分别是这个房间存放的食物和所需要的猫粮。求这组数据能保证的最大的食物是多少？ （可以不完全保证这个房间的食物，及食物和猫粮可以同时乘a%） 
**经典的贪心策略**。 
*先保证性价比最高的房间（花较少的猫粮可以保证最多的粮食），每组数据计算一个比率rate = 房间存放的粮食/所需要的猫粮，按照rate对其进行降序排列，优先满足上方的房间即可。*

```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
struct room{
	double food;
	double need;
	double rate;
}item[1010];
double cmp(room x,room y){
	return x.rate>y.rate;
}
int main(){
	int t,i;
	double a;
	while(cin>>a>>t&&(a!=-1||t!=-1)){
		for(i=0;i<t;i++){
			cin>>item[i].food>>item[i].need;
			item[i].rate=item[i].food/item[i].need; //rate越大越划算
		}
		double ans=0;
		sort(item,item+t,cmp);
		for(i=0;i<t;i++){
			if(a>item[i].need){
				ans+=item[i].food;
				a-=item[i].need;
			}
			else{
				double percentage=a/item[i].need;
				ans+=item[i].food*percentage;
				break;
			}
		}
		printf("%.3lf\n",ans);
	}
	return 0;
}

```