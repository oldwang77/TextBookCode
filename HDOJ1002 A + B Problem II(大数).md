[HDOJ 1002](http://acm.hdu.edu.cn/showproblem.php?pid=1002)
>解题方法：
     先将字符串反转存入整形数组，整形数组按位相加，若相加后大于十则前一位加一。相加结束后，倒叙逐个输出整形数组的每一位数。
```
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int max(int x,int y)
{
	int z;
	z=x>y?x:y;//比较两个值大小 
	return z;
}

int main()
{
	int lena,lenb,i,n,t=0,z,numa[1100],numb[1100];
	char a[1100],b[1100];
	scanf("%d",&n);
	while(n--)
	{
		memset(numa,0,sizeof(numa));
		memset(numb,0,sizeof(numb));//int数组清零 
		scanf("%s%s",a,b);
		lena=strlen(a);
		lenb=strlen(b);
		for(i=0;i<lena;i++)
		   numa[lena-1-i]=a[i]-'0';
		for(i=0;i<lenb;i++)
		   numb[lenb-1-i]=b[i]-'0';
		z=max(lena,lenb);
		for(i=0;i<z;i++)//此处i值的范围就是两个字符串长度的最大者
		{
			numa[i]=numa[i]+numb[i];
			if(numa[i]>9)//处理相加大于十的情况
			{
				numa[i]=numa[i]-10;
				numa[i+1]++;
		    }
		}
		t++;
		printf("Case %d:\n",t);  
        printf("%s + %s = ",a,b);  
 
		if(numa[z]==0)// 判断数组第z位是否为零 
		{
			for(i=z-1;i>=0;i--)
			   printf("%d",numa[i]);
			printf("\n");
		}
		else
		{
			for(i=z;i>=0;i--)
			  printf("%d",numa[i]);
			printf("\n");
		}
		if(n!=0)//每一次的输入遇上一次的输出之间空一行 ，n=0是输出的是最后一组数据，即不用空出一行 
		  printf("\n");
	}
	return 0;
}
```
