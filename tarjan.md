```c++
void tarjan(int x){//tarjan 回溯法求强连通分量

	low[x]=dfn[x]=++id;
	stac[++top]=x;
	pd[x]=1;

	for(int i=head[x];i!=0;i=e[i].next){
		int v=e[i].to;
		if(dfn[v]==0){
			tarjan(v);
			low[x]=min(low[x],low[v]);
		}else if(pd[v]==1){
			low[x]=min(low[x],low[v]);
		}
	}

	if(low[x]==dfn[x]){
		tot++;
		while(stac[top+1]!=x){
			sum[tot]+=power[stac[top]];
			pd[stac[top]]=0;
			color[stac[top--]]=tot;
		}
	}

}
```

```c++
void tarjan(int x,int rt){//tarjan回溯法求割点

	dfn[x]=low[x]=++id;//dfn,low节点值更新
	int child=0;
	int size=g[x].size();
	for(int i=0;i<size;i++){
		int v=g[x][i];//子节点
		if(dfn[v]==0){
			tarjan(v,rt);//dfs深搜tarjan
			low[x]=min(low[x],low[v]);//更新low节点
			if(low[v]>=dfn[x]&&x!=rt){//子节点无法回溯到比父节点更加早地位置
				cut[x]=1;
			}
			if(x==rt){//根节点子节点记数
				child++;
			}
		}
		low[x]=min(low[x],dfn[v]);//如果撞墙更新子节点
	}

	if(child>=2&&x==rt){//有两个子节点的根节点是割点
		cut[x]=1;
	}

}
```

```c++
vector<vector<int>> g;
set<pair<int,int>> st;
pair<int,int> stac[N];
set<int> bcc[N];
vector<int> color[N];
bool cut[N];
int top;
int dfn[N],low[N],id;
int tot;
void tarjan(int x,int fa,int rt){//通过求割点求点双连通分量

	int child=0;//根节点通过其子节点个数判断是否为割点
	dfn[x]=low[x]=++id;//初始化dfn,low数组
	int size=g[x].size();

	if(x==rt&&size==0){//孤立点特判为特殊的点双连通分量以将其与其他的孤立点区别
		tot++;
		bcc[tot].insert(x);
		color[x].push_back(tot);
	}

	for(int i=0;i<size;i++){
		int v=g[x][i];
		if(v!=fa){
			
			if(dfn[v]==0){//v为未被遍历过的点
				
				stac[++top]=make_pair(x,v);//将边压入栈
				tarjan(v,x,rt);//继续深搜
				child++;//增加子节点个数
				low[x]=min(low[x],low[v]);//更新low值
				if(low[v]>=dfn[x]){//无法回溯到前节点，为割点或根节点，出现双连通分量
					tot++;//双连通分量数量加一
					cut[x]=1;//先判定为割点，若为非割点的根节点后面特殊处理
					pair<int,int> edge;
					while(!(stac[top+1].first==x&&stac[top+1].second==v)){//将点双连通分量中的边输出
						edge=stac[top--];//弹出边
						color[edge.first].push_back(tot);//将双连通分量放入vector中
						color[edge.second].push_back(tot);
						bcc[tot].insert(edge.first);//将点放入双连通分量中
						bcc[tot].insert(edge.second);
					}
				}

			}else if(dfn[v]!=0){//撞墙，更新low值
				low[x]=min(low[x],dfn[v]);
			}

		}
	}

	if(x==fa&&child<2){//若根节点子节点不足两个，cut置为0
		cut[x]=0;
	}

}
```

```c++
void tarjan(int x,int fa){//回溯法求割边

	dfn[x]=low[x]=++id;//更新dfn,low节点
	int size=g[x].size();
	int k=0;//重边标记
	for(int i=0;i<size;i++){
		int v=g[x][i];
		if(fa!=v||k==1){
			if(dfn[v]==0){
				tarjan(v,x);//继续深搜
				low[x]=min(low[x],low[v]);//更新low值
				if(low[v]>dfn[x]){//low v > dfn x 找到割边
					cut[x][i]=1;
					flag=1;
				}
			}
			low[x]=min(low[x],dfn[v]);//撞墙回溯
		}else{
			k=1;
		}
	}

}
```

