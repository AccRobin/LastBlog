---
title: CF535D Tavas and Malekas 题解
tag: 字符串
---
# CF535D Tavas and Malekas 题解

> 不要看它1900就以为这是一道水题

[题目链接](http://codeforces.com/problemset/problem/535/D)

## 题意

　　给定文本串的长度$n$和一个模式串$S$， 并且已知模式串在文本串中某些出现位置（不一定仅在这些位置出现）， 求可能的文本串的数量。

## 思路

　　看似数数题，但事实上只需要考虑有哪些位置是“自由”（即可以是`a`到`z`中任意一个字母）的，记为$cnt$，那么答案就是$26^{cnt}$.本题的难点在于判无解的情况，即相邻两个模式串可能会冲突.

## 解决方案

　　这题用KMP做更合适，但为了练习Z函数，故此处使用Z函数.（事实上，KMP和Z函数没有什么区别）.

　　我们预处理出模式串的Z函数.

　　记相邻的两个模式串为$a,b$，不妨设$a$在$b$的前面.

　　很显然，相邻两个模式串会冲突当且仅当下面两个条件同时满足

　　1.这两个模式串有交集

　　2.交集部分不是$a$的后缀或不是$b$的前缀.

　　那么由Z函数的定义可知，上面两个条件形式化的表达就是

$$
a_i-a_{i-1}<len ~ \land  z[a_i-a_{i-1}]<len-(a_i-a_{i-1})
$$

　　其中$a_i$表示第$i$个模式串首位出现的位置，$len$表示模式串的长度.

　　只需要逐一比较即可.

## 代码

```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
namespace Acc{
	const int N = 1e6+10;
	const int mod = 1e9+7;
	char s[N];
	int n,m,a[N],z[N],i,l,r,len,ans,sum[N];
	int qpow(int a,int b){
		int res=1;
		for(;b;a=1ll*a*a%mod,b>>=1)if(b&1)res=1ll*res*a%mod;
		return res;
	}
	void work(){
		for(scanf("%d%d%s",&n,&m,s),i=1;i<=m;i++)scanf("%d",&a[i]);
		for(len=strlen(s),i=1,l=0,r=0;i<len;i++){
			if(i<=r&&z[i-l]<r-i+1) z[i]=z[i-l];
			else for(z[i]=max(0,r-i+1);i+z[i]<n&&s[z[i]]==s[i+z[i]];)z[i]++;
			if(i+z[i]-1>r) l=i,r=i+z[i]-1;
		}
		for(sort(a+1,a+m+1),i=2;i<=m;i++)if(a[i]-a[i-1]<len&&z[a[i]-a[i-1]]<len-a[i]+a[i-1])return (void)(printf("0"));
		for(i=1;i<=m;i++)sum[a[i]]++,sum[a[i]+len]--;
		for(int i=1;sum[i]+=sum[i-1],i<=n;i++)if(!sum[i])ans++;
		printf("%d",qpow(26,ans));
	}
}
int main(){
	return Acc::work(),0;
}		
```