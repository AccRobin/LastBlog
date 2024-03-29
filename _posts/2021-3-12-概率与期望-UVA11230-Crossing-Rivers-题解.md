---
title: Crossing Rivers 题解
tag: 数学 概率与期望
---
# UVA11230 Crossing Rivers 题解

这道题好像大部分洛谷的题解都是不规范的。

## 题意
您住在一个小村庄，但是您工作的地方在另一个村庄。您上班的路程是通过你家 $(A)$和你公司$(B)$的一条直路。但是有一些河流将这条路拦腰截断。我们假设 $B$在 $A$的右边，所有的河流都在这两者中间。

幸运的事，这里有一个匀速行驶的"过河自动机"（自动船）.当你到达河的左岸时，仅需等船，然后上船，然后就到对岸了。我们假设您的质量非常轻，上船后船的速度并不会改变。

往事缱绻，光阴荏苒，您灵机一动，提出了如下的问题：

假设所有的船在时刻$1$都在河流的任意位置，那么从$A$到$B$的期望时间是多少？您的走路速度为$1$.

更加严谨的说,对于一条长为$L$的河流,时刻$0$时船与左岸的距离为$[0,L]$.

输出期望时间.

## 题解

此题用平均数的做法是不够严谨的。

对于这种稠密的期望，我们应当通过积分来做。

设任意时刻，小船的位置在$x~(x \in [0,L])$，渡河的期望时间为$E(T)$，小船到岸边的期望距离为$E(X)$，则有

$$
E(T)=\frac {E(X)}v \\
$$

对于$E(X)$，有

$$
E(X)=\int_0^{2L} \frac 1 L \times x ~ dx  \\
=\left. \frac{x^2}{2L} \right|_0^{2L} \\
=2L
$$

所以，题目所求即为

$$
E(T)=\frac{2L}v
$$

```cpp
//UVA12230 Crossing Rivers
//2021-3-12
#include<cstdio>
#define db double
namespace Acc{
	int n,d,p,l,v,kase;
	int work(){
		while(scanf("%d%d",&n,&d)){
			if(!n && !d) return 0;
			db ans=0.0;
			for(int i=1;i<=n;i++){
				scanf("%d%d%d",&p,&l,&v);
				ans+=(db)2*l/v;
				ans-=l;
			}
			ans+=d;
			printf("Case %d: %.3lf\n\n",++kase,ans);
		}
		return 0;
	}
}
int main(){
	return Acc::work();
}
```