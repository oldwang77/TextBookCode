[HDOJ 1004](http://acm.hdu.edu.cn/showproblem.php?pid=1004)
这是一道字符串的题目。题目就是输入N个气球的颜色，然后找出其中出现颜色最多的气球。
很自然的想到map，通过map很轻松就能统计出各个气球出现的次数，通过迭代器遍历容器。
这道题和HDOJ 2072类似。
```
#include<iostream>
#include<map>
#include<string>
using namespace std;
int main(){
	int n;
	string s;
	map<string,int > balloon_colour;  //建立map的映射
	map<string,int >::iterator it;
	while(scanf("%d",&n)!=EOF){
		if(n==0) break;
		while(n--){
			s="";  //每记录一个单词前，要还原
			getline(cin,s);   //字符串读取一行时，用getline比较好
			balloon_colour[s]++;
		}
		int max=0,i; 
		string l;
		for(it=balloon_colour.begin ();it!=balloon_colour.end ();++it)
		{
			if(balloon_colour[it->first]>max)  //对应的value大于max
				l=it->first;		
		}
	cout<<l<<endl;
	balloon_colour.clear ();
	}
	return 0;
}
```