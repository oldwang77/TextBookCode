[HDOJ 2072](http://acm.hdu.edu.cn/showproblem.php?pid=2072)
这是一道字符串相关的题目。题意就是输入一段话，每段话一行，统计这段话中有多少个单词。
很容易想到map（可以简单的认为是有序的二维数组），将不同的单词和出现的次数，记录在`map<string ,int > words`中，然后通过建立迭代器`map<string,int >::iterator it`; 访问key值和对应value值。
这里还用到了c++中string类型。不同于c，c++中的string类型可赋值，加减，比如这里的char ch; string s; s+=ch;
```
#include<iostream>
#include<map>
#include<string>
using namespace std;
int main(){
	string s;
	int n;
	char ch;
	while(scanf("%c",&ch)!=EOF&&ch!='\n'){
		if(ch=='#') break; //#退出
			map<string,int > words;  //建立map的映射
		n=0;
		while(1)
		{
			if(ch>='a'&&ch<='z')
				s+=ch;
			if(ch==' '||ch=='\n') //遇到空格或回车表示一个单词读取完毕
				words[s]++;  //此对应对应的映射
			if(ch=='\n') break;
				scanf("%c",&ch);
		}
		map<string ,int >::iterator it; //容器迭代器
		for(it=words.begin();it!=words.end();++it)
		{
			n++;
		//	cout<<it->first<<" ";  打印key的值
		//	cout<<it->second<<" ";	打印value的值
		}
		cout<<n<<endl;
		words.clear(); //养成容器用后清理的习惯
	}
	return 0;
}
```

