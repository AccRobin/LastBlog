---
title: CF433C Ryouko's Memory Note 题解
tag: 随心刷题
---
# CF433C Ryouko's Memory Note 题解

> 不要看它1800就以为这是一道水题

[题目链接](https://codeforces.com/problemset/problem/433/C)

## 题意

　　给出$2$个正整数$m,n(1\le n,m \le 10^5)$，和$n$个数$a_1,a_2,\cdots a_n(1\le a_i\le m)$你可以把这个序列里所有值为$a_i$的元素改成$a_j(1\le i,j\le m)$，$i$可以等于$j$，只可以改一次.

　　最小化$\sum\limits_{i=1}^{i\le m-1} \mid a_i-a_{i+1} \mid$.

## 思路

　　观察到，只改变一个元素的值对于答案的贡献并不广泛.更加具体地，若我们打算改变$k$的值，那么对于答案的影响只和**和$k$相邻且不等于$k$的元素**有关.

　　假设我们提取出了所有**和$k$相邻且不等于$k$的元素**，并准备改变$k$的值.记所有**和$k$相邻且不等于$k$的元素**为$b_i(0 \le i \le s)$，那么题目所求即为

$$
\sum_{0\le i \le s}\mid k-b_i \mid
$$

　　只需最小化上式即可.

## 解决方案

　　枚举每一个$a_i$，取得所有和$a_i$相邻且不等于$a_i$的元素.对于式

$$
\sum_{0\le i \le s}\mid k-b_i \mid，
$$

有一个显而易见的结论是：当$a_i$为序列$\{b\}$的中位数时，上式取得最小值.

　　最后比较并统计答案即可.

## 复杂度分析

　　复杂度较难分析，但大致可以得到一个严格的上界：

　　枚举$a_i$的复杂度为$O(m)$，取得中位数的复杂度为$O(n)$，总复杂度$O(m \log n)$.

## 代码

　　稍微压了一下，但大致不影响阅读.

```cpp
#include<cstdio>
#include<string>
#include<algorithm>
namespace Acc{
	std::basic_string<int>G[100009];
	int a[100009],n,m,i,j,k;
	long long res1,res2,ans=1ll<<40,sum=0;
	int work(){
		for(scanf("%d%d",&m,&n),i=1;i<=n;i++)scanf("%d",&a[i]);
		for(a[0]=a[1],a[n+1]=a[n],i=1;i<=n;sum+=abs(a[i]-a[i-1]),i++){
			if(a[i-1]!=a[i])G[a[i]]+=a[i-1];
			if(a[i]!=a[i+1])G[a[i]]+=a[i+1];
		}
		for(ans=sum,i=1;i<=m;i++)if(!G[i].empty()){
				for(std::sort(G[i].begin(),G[i].end()),k=G[i][G[i].size()/2],res1=res2=j=0;j<(int)G[i].size();j++)res1+=abs(G[i][j]-k),res2+=abs(G[i][j]-i);
				ans=std::min(ans,sum-res2+res1);
			}
		printf("%lld",ans);
	}
}
int main(){
	return Acc::work();
}
```