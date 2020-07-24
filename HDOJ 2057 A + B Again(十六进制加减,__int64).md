[HDOJ 2057](http://acm.hdu.edu.cn/showproblem.php?pid=2057)
这是一道关于__int64 ,十六进制的题目。
说明：%I64X：用大写字母输出无符号的十六进制数，也就是说无法输出负数。一个简单的写法，负数取绝对值，加个负号即可。
下面转载了一段关于__int64的用法说明：

[__int64的使用方法](http://blog.csdn.net/shiwei408/article/details/7463476)

```
#include<iostream>
using namespace std;
int main()
{
	__int64 m,n;
	while(scanf("%I64X%I64X",&m,&n)){
	m=m+n;
	if(m<0){
		m=-m;
		printf("-%I64X\n",m);
	}
	else
		printf("%I64X\n",m);
	}
	return 0;
}
```