Jobdu1201 二叉排序树
题目描述： 
    输入一系列整数，建立二叉排序数，并进行前序，中序，后序遍历。
输入： 
    输入第一行包括一个整数n(1<=n<=100)。
    接下来的一行包括n个整数。
输出： 
    可能有多组测试数据，对于每组数据，将题目所给数据建立一个二叉排序树，并对二叉排序树进行前序、中序和后序遍历。
    每种遍历结果输出一行。每行最后一个数据之后有一个空格。
样例输入： 
5
1 6 5 9 8
样例输出： 
1 6 5 9 8 
1 5 6 8 9 
5 8 9 6 1 
提示： 
输入中可能有重复元素，但是输出的二叉树遍历序列中重复元素不用输出。
来源： 
2005年华中科技大学计算机保研机试真题 

```
#include<iostream>
#include<cstring>
using namespace std;
struct Node{
	Node *lchild;
	Node *rchild;
	char c;
}Tree[110];
int loc;    //使用元素的个数
Node *creat(){ //申请未使用的结点
	Tree[loc].lchild=Tree[loc].rchild=NULL;
	return &Tree[loc++];
}
void pre_order(Node *T){
	printf("%d ",T->c);
	if(T->lchild!=NULL) pre_order(T->lchild);
	if(T->rchild!=NULL) pre_order(T->rchild);
}
void in_order(Node *T){
	if(T->lchild!=NULL) in_order(T->lchild);
	printf("%d ",T->c);
	if(T->rchild!=NULL) in_order(T->rchild);
}
void post_order(Node *T){
	if(T->lchild!=NULL) post_order(T->lchild);
	if(T->rchild!=NULL) post_order(T->rchild);
	printf("%d ",T->c);
}
Node *Insert(Node *T,int x){
	if(T==NULL){   //T是根结点
		T=creat();
		T->c=x;
		return T;
	}
	else if(x<T->c)
		T->lchild=Insert(T->lchild,x); 
	else if(x>T->c)
		T->rchild=Insert(T->rchild,x);
		//插入右子树上，若根结点数值和X一样，按题目要求，直接忽略
	return T;
}

int main(){
	int n;
	while(scanf("%d",&n)!=EOF){
		loc=0;
		Node *T=NULL;
		int i;
		for(i=0;i<n;i++){
			int x;
			scanf("%d",&x);
			T=Insert(T,x); //把T插入排序树中
		}
		pre_order(T);
		printf("\n");
		in_order(T);
		printf("\n");
		post_order(T);
		printf("\n");
	}
	return 0;
}
```