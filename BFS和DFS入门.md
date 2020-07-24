摘自：
[BFS入门](http://blog.csdn.net/raphealguo/article/details/7523411)
BFS的核心代码：
```
bool BFS(Node& Vs, Node& Vd){  
    queue<Node> Q;  
    Node Vn, Vw;  
    int i;  
  
    //用于标记颜色当visit[i][j]==true时，说明节点访问过，也就是黑色  
    bool visit[MAXL][MAXL];  
  
    //四个方向  
    int dir[][2] = {  
        {0, 1}, {1, 0},  
        {0, -1}, {-1, 0}  
    };  
  
    //初始状态将起点放进队列Q  
    Q.push(Vs);  
    visit[Vs.x][Vs.y] = true;//设置节点已经访问过了！  
  
    while (!Q.empty()){//队列不为空，继续搜索！  
        //取出队列的头Vn  
        Vn = Q.front();  
        Q.pop();  
  
        for(i = 0; i < 4; ++i){  
            Vw = Node(Vn.x+dir[i][0], Vn.y+dir[i][1]);//计算相邻节点  
  
            if (Vw == Vd){//找到终点了！  
                //把路径记录，这里没给出解法  
                return true;//返回  
            }  
  
            if (isValid(Vw) && !visit[Vw.x][Vw.y]){  
                //Vw是一个合法的节点并且为白色节点  
                Q.push(Vw);//加入队列Q  
                visit[Vw.x][Vw.y] = true;//设置节点颜色  
            }  
        }  
    }  
    return false;//无解  
}  
```

[DFS入门](http://blog.csdn.net/raphealguo/article/details/7560918)
DFS的核心代码：

```
bool DFS(Node n,int d ){//d是深度，假设要求深度为4
	if(isEnd(n,d)){  //一旦到了摸个状态，路径返回true,证明本次搜索有解
		return true;
	}
	
	for(Node nextNode ,int n){//遍历和节点n相邻的节点nextNode
		if(!visit[nextNode]){
			visit[nextNode]=true;	//例如搜索到了V1，将V1设为已经访问
		
				if(DFS(nextNode,d+1)){//如果搜索出有解
					//做些其他事情，比如记录深度等
					//例如到了V6，找到解了，你必须一层一层的告苏上面找到解
					return true;
				}
			//重新设置成未访问，因为它和可能出现在下一次搜索的别的路径中
				visit[nextNode]=false;
		}
	}
	return false;
}
```