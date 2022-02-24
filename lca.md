```c++
void dfs(int x,int fath){//深搜建立倍增节点
	fa[x][0]=fath;depth[x]=depth[fath]+1;//x的2^0级祖先即为其父节点
	for(int i=1;i<=20;i++){//x的2^i级祖先就是x的2^(i-1)级祖先的2^(i-1)级祖先
		fa[x][i]=fa[fa[x][i-1]][i-1];
	}
	int size=tree[x].size();
	for(int i=0;i<size;i++){
		int v=tree[x][i];
		if(v!=fath){
			dfs(v,x);//继续深搜
		}
	}
}

int lca(int x,int y){

	if(depth[x]<depth[y]){//确保x比y要深
		swap(x,y);
	}
	while(depth[x]!=depth[y]){//将x抬到与y同一水平线上
		x=fa[x][lg[depth[x]-depth[y]]-1];
	}
	if(x==y)return x;
	for(int i=20;i>=0;i--){
		if(fa[x][i]!=fa[y][i]){//不相等就是没有到公共祖先节点，上升
			x=fa[x][i];y=fa[y][i];
		}
	}
	return fa[x][0];//因为只有不相等才能上升无法直接到达公共祖先节点，所以只能到达x节点的父节点
}

for(int i=1;i<=n;i++){
	lg[i]=lg[i-1]+(1<<lg[i-1]==i);
}
```

