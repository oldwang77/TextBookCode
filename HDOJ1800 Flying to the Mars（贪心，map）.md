开始输入用的cin，超时了，还是用scanf，AC。
实际上需要的扫帚的个数，就是一个队伍中等级相同的人数最多的那一组的人数，用map统计出相同人数最多的那一组即可。
>有n个人，每个人都有一定的等级，高等级的人可以教低等级的人骑扫帚，并且他们可以共用一个扫帚，问至少需要几个扫帚。 
这道题与最少拦截系统有异曲同工之妙。不同在于这道题可以排序，而最少拦截系统不能排序。我们想一下，把这些人排好序，并统计每个等级的人的个数。一次从最高等级到最低等级划掉一个人（可以理解为这些人互相教并且骑一个扫帚），一直划，直到最后一个人。这样可以看出来，扫帚的数量肯定是由某个等级的人数确定的。并且那个等级的人数最多。因此，只要统计出等数最多的等级就好，最后输出那个等级的人数。我采用了map映射，这样较为简单。
```
#include <iostream>
#include <cstdio>
#include <cstring>
#include<map>
#include <algorithm>
using namespace std;
const int maxn=3010;
map<int ,int > p;
map<int ,int >::iterator iter;

int main(){
	int n;
	while(scanf("%d",&n)!=EOF){
		int i,temp;
		for(i=0;i<n;i++){
			scanf("%d",&temp);
			p[temp]++;
		}
		int nmax=0;
		for(iter=p.begin();iter!=p.end();++iter){
			if(iter->second>nmax){
				nmax=iter->second;
			}
		}
		p.clear();
		printf("%d\n",nmax);
	}
	return 0;
}

```
