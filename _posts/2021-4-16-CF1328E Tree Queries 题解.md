---
title: CF1328E Tree Queries 题解
tag: 随心刷题
---
# CF1328E Tree Queries 题解

> 不要看它`*1800`就以为这是一道水题

[题目链接](http://codeforces.com/problemset/problem/1328/E)

## 题意

　　给你一个以$1$为根的有根树，每回询问$k$个节点$v_1, v_2 \cdots v_k$，
求出是否有一条以根节点为一端的链使得询问的每个节点到此链的距离均$\leq 1$.

只需输出可行性, 无需输出方案.

## 思路

　　发现每个节点到这条链的距离小于等于$1$在树上有很多奇妙的性质，而且小于等于$1$可以分为两种情况，分别考虑一下.

## 解决方案

　　对于小于$1$和等于$1$分类

- 小于$1$的情况：即该链经过这个点.因为这条链的一端是根，那么这个链必然经过该点和该点的祖先.

- 等于$1$的情况：在树上的一条一端是根的链就想要到一个点的距离为$1$只有一种可能，就是经过这个点的父亲.

　　我们发现这两种情况下，这条链都经过了这些选中的点的父亲，那么问题就转化为：

　　是否存在一条一端是根的链，经过所有选中点的父亲.至此，这个问题已经很好处理了.

　　先取出所有的选中点$v_1,v_2,\cdots v_k$的父亲$fa[v_1],fa[v_2],\cdots fa[v_k]$，记为$a_1,a_2,\cdots,a_k$。

　　按照深度对这些点排序，然后用$dfn$序判断每一对相邻的两个节点，深度较大的节点是否在深度较小的节点的子树里即可.

## 代码

```cpp
//CF1328E Tree Queries
//2021-3-29
#include<cstdio>
#include<string>
#include<algorithm>
namespace Acc{
	const int N = 2e5+10;
	int dfn[N],dfn2[N],n,k,m,a[N],d[N],i,j,u,v,f,tm,fa[N];
	std::basic_string<int>G[N];
	void dfs(int u){int v,i;
		for(dfn[u]=++tm,i=0;i<G[u].size();i++)
			if(!d[v=G[u][i]])fa[v]=u,d[v]=d[u]+1,dfs(v);
		dfn2[u]=++tm;
	}
	bool cmp(int x,int y){return d[x]<d[y];}
	void work(){
		for(scanf("%d%d",&n,&m),i=1;i<n;i++)
			scanf("%d%d",&u,&v),G[u]+=v,G[v]+=u;
		for(fa[1]=d[1]=1,dfs(1),i=1;f=0,i<=m;i++){
			for(scanf("%d",&k),j=1;j<=k;j++) 
				scanf("%d",&a[j]),a[j]=fa[a[j]];
			for(std::sort(a+1,a+k+1,cmp),j=1;j<k;j++)
				if(!(dfn[a[j]]<=dfn[a[j+1]]&&dfn[a[j+1]]<=dfn2[a[j]])){
					printf("NO\n"),f=1;break;
				}
			if(!f)printf("YES\n");
		}
	}
}
int main(){
	return Acc::work(),0;
}	
```