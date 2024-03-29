---
title: Falling Anvils 题解
tag: 数学 概率与期望
---
# CF77B Falling Anvils 题解

## 题目描述

题目链接:[CF77B](http://codeforces.com/problemset/problem/77/B)

题意：

　　给定实数$a, b(0\le a, b\le 10^6)$，求方程$x^2+\sqrt px +qx=0$有至少一个实根的概率，其中$p\in [0, a]$, $q\in [-b, b]$，$p$, $q$均是实数，在上述区间内等概率分布。

## 解决方案

　　易知若原方程有至少一个实根，当且仅当

$$
\Delta=p-4q \ge 0
$$

　　令$x=4q,y=p$，$x\in[-4b,4b],y\in[0,a]$，则

$$
y-x\ge 0
$$

　　以$b=2$，$a=4$为例作出图像，有

![](https://accrobin.github.io/assets/image/CF77B/001.jpg)

　　其中矩形$ABCD$表示$x$和$y$的取值范围，而四边形$ABOE$表示在$x$和$y$的取值范围下满足$y-x \ge 0$的部分.

　　因为$x$和$y$在取值范围内均匀分布，所以$\frac{S_{ABOE}}{S_{ABCD}}$即为最终答案.

　　本题注意分类讨论！

## 代码：

```cpp
//CF77B Falling Anvils
//2021-3-13
#include<cstdio>
#include<algorithm>
namespace Acc{
	long long a,b;
	void work(){
		scanf("%lld%lld",&a,&b);
		if(!b) {
			printf("1\n");
			return;}
		if(!a){
			printf("0.5000000000\n");
			return;
		}
		printf("%.10f\n",(double)(std::max(0ll,a-4*b)+a)*std::min(a,4*b)/(16*a*b)+0.5);
	}
}
int main(){
	int T;
	scanf("%d",&T);
	while(T--) Acc::work();
	return 0;
}
```