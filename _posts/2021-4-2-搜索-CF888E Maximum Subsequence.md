---
title: CF888E Maximum Subsequence 题解
tag: 随心刷题
---
# CF888E Maximum Subsequence 题解

> 不要看它1800就以为这是一道水题

[题目链接](https://codeforces.com/problemset/problem/888/E)

## 题意

　　给一个数列和$m$，在数列任选若干个数，使得他们的和模$m$最大.（ $1\le n\le 35,1\le m\le 10^{9}$） 

## 思路

　　看到$n\le 35$就知道这是一道搜索题（有经验的一看就知道是折半）.

　　考虑枚举子集，但$O(2^n)$过不去，考虑`Meet in the middle`.


## 解决方案.

　　枚举子集时分为两半，每一半是$O(2^{\frac n2})$.合并时显然不能暴力合并，我们小贪一波.

　　记两个枚举出的序列记为$A$和$B$，我们分别对这两个序列排序，排序后双指针从两侧向中间逼近即可.

## 代码

```cpp
//CF888E Maximum Subsequence
//2021-4-1
#include<cstdio>
#include<algorithm>
#define ll long long
using namespace std;
namespace Acc{
	const int N = 3e5+10;
	ll a[N],b[N],i,j,s[40],n,mod,res,ans;
	void work(){
		for(scanf("%lld%lld",&n,&mod),i=0;i<n;i++)scanf("%lld",&s[i]);
		for(i=0;i<(1ll<<(n/2));i++){
			for(res=0,j=0;j<n;j++) 
				if(i&(1ll<<j)) res=(res+s[j])%mod;
			a[i]=res;
		}
		for(i=0;i<(1ll<<(n/2+(n&1)));i++){
			for(res=0,j=0;j<n;j++) if((i<<(n/2))&(1ll<<j)) res=(res+s[j])%mod;
			b[i]=res;
		}
		sort(a,a+(1ll<<(n/2)));
		sort(b,b+(1ll<<(n/2+(n&1))));
		for(j=(1ll<<(n/2+(n&1)))-1,i=0;i<(1ll<<(n/2));i++){
			while(a[i]+b[j]>=mod)j--;
			ans=max(ans,a[i]+b[j]);
		}
		ans=max(ans,a[(n>>1)-1]+b[(n/2+(n&1))-1]-mod);
		printf("%lld",ans);
	}
}
int main(){
	return Acc::work(),0;
}
```