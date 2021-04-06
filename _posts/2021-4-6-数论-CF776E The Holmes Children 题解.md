---
title: CF776E The Holmes Children 题解
tag: 数学 数论
---
# CF776E The Holmes Children 题解

> 不要看它`2100`就以为这是一道水题

[题目链接](https://codeforces.com/problemset/problem/776/E)

## 题意

　　定义数论函数

$f$:

$$
f(1)=1\\

f(n)=\sum_{i=1}^{n-1}[\gcd(i,n-i)=1]\\
$$

$g$:

$$
g(n)=\sum_{d\mid n}f(\frac nd)
$$

$F_k(n)$:

$$
F_k(n)=
\left\{\begin{matrix}
f(g(n))  &  n=1\\
g(F_{k-1}(n))  & 2\mid n\\
f(F_{k-1}(n))  & 2\nmid n\\
\end{matrix}\right.
$$

　　给定$n,k$，求$F_k(n)$.

## 思路

　　$f$看着就像$\varphi$，$g$一看就是个卷积，直接推式子.

## 解决方案

　　由最大公约数的性质可知，

$$
\gcd(i,n-i)=\gcd(i,n).
$$

　　故$f$为

$$
f(n)=\sum_{i=1}^{n-1}[\gcd(i,n)=1]=\varphi(n).
$$

　　由Dirichlet卷积的知识可得

$$
g(n)=\sum_{d\mid n}f(d)=\\\sum_{d\mid n}\varphi(d)=(\varphi*1)(n)=id(n)=n.
$$

　　则$F_k$函数的意义即为 对自变量$n$迭代$\lfloor \frac{k+1}2 \rfloor$次欧拉函数.

　　由欧拉函数的性质可知，一个数$n$至多迭代$\log n$次就会变为$1$.

　　所以我们可以一直迭代，直到$\lfloor \frac{k+1}2 \rfloor$次迭代结束或者已经变成了$1$.

## 复杂度分析

　　单独求一个数的欧拉函数复杂度为$O(\sqrt n)$.

　　故总复杂度为为$O(\min(\lfloor \frac{k+1}2 \rfloor,\log n) \cdot \sqrt n)$.

## 代码

```cpp
#include<cstdio>
#define ll long long 
using namespace std;
namespace Acc{
	const int mod = 1e9+7;
	ll phi(ll x){
		ll ans=x;
		for(ll i=2;i*i<=x;i++){
			if(!(x%i))ans=ans/i*(i-1);
			while(!(x%i))x/=i;
		}
		if(x>1)ans=ans/x*(x-1);
		return ans;
	}
	ll k,n;
	void work(){
		scanf("%lld%lld",&n,&k);
		k=(k+1)/2;
		while(k){
			n=phi(n);
			k--;
			if(n==1) return (void)(printf("1"));
		}
		printf("%lld",n%mod);
	}
}
int main(){
	return Acc::work(),0;
}
```