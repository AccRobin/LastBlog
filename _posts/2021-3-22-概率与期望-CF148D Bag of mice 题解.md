---
title: Bag of mice 题解
tag: 数学 概率与期望 题解
---
# CF148D Bag of mice 题解

## 题目描述

题目链接：[CF148D](http://codeforces.com/problemset/problem/148/D)

题意：

　　袋子里有$a$只白鼠和$b$只黑鼠 ，A和B轮流从袋子里抓，谁先抓到白色谁就赢。A每次随机抓一只，B每次随机抓完一只之后会有另一只随机老鼠跑出来。如果两个人都没有抓到白色则B赢。A先抓，问A赢的概率。

## 解决方案

　　考虑概率DP.

　　设$f[i][j]$表示剩余$a$只白鼠，$b$只黑鼠的情况下，A赢的概率.

　　分类讨论如下：

　　1.A直接拿一只白鼠，概率为$\frac i{i+j}$；

　　2.A拿了一只黑鼠，概率为$\frac j{i+j}$：

　　　(1).B拿了白鼠，概率为$\frac i{i+j-1}$；

　　　(2).B拿了黑鼠，概率为$\frac{j-1}{i+j-1}$：

　　　　1>跑了一只白鼠，概率为$\frac{i}{i+j-2}$;

　　　　2>跑了一只黑鼠，概率为$\frac{j-2}{i+j-2}$.

　　其中1.(1)对答案没有贡献.

　　故转移方程为：

$$

f[i][j]=\frac i{i+j}+\frac j{i+j}\times (\frac{j-1}{i+j-1}\times (\frac{i}{i+j-2}\times f[i-1][j-2]+\frac{j-2}{i+j-2}\times f[i][j-3])).\\

$$

　　特殊情况：

　　1.$f[i][0]=1$.

　　2.$f[0][i]=0$.

## 代码

```cpp
//CF148D Bag of mice
//2021-3-13
#include<cstdio>
#define db double

namespace Acc{
	const int N = 1e3+10;
	db f[N][N];
	int a,b;
	int work(){
		scanf("%d%d",&a,&b);
		f[0][0]=0;
		for(int i=1;i<=a;i++) f[i][0]=1.0;
		for(int i=1;i<=b;i++) f[0][i]=0.0;
		for(int i=1;i<=a;i++){
			for(int j=1;j<=b;j++){
				f[i][j]=(db)i/(i+j);
				if(j-2>=0) f[i][j]+=(db)j/(i+j)*(db)(j-1)/(i+j-1)*(db)i/(i+j-2)*f[i-1][j-2];
				if(j-3>=0) f[i][j]+=(db)j/(i+j)*(db)(j-1)/(i+j-1)*(db)(j-2)/(i+j-2)*f[i][j-3];
			}
		}
		printf("%.9lf",f[a][b]);
		return 0;
	}
}
int main(){
	return Acc::work();
}
```