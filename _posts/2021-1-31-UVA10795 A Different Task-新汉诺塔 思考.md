## UVA10795 A Different Task-新汉诺塔 思考

```cpp
//UVA10795 A Different Task
#include<cstdio>
#define ll long long
namespace Acc
{
	const int N = 70;
	int begin[N],end[N];
	int cnt=1;
	int n;
	
	ll f(int *p,int i,int final)
	{
		if(i==0) return 0;
		if(p[i]==final) return f(p,i-1,final);
		return (1LL<<(i-1))+f(p,i-1,6-final-p[i]);
	}
	
	int work()
	{
		while(scanf("%d",&n) && n)
		{
			for(int i=1;i<=n;i++) scanf("%d",&begin[i]);
			for(int i=1;i<=n;i++) scanf("%d",&end[i]);
			int k=n;
			while(k>=1 && begin[k]==end[k]) k--;
			ll ans=0;
			if(k>0){
				ans=f(begin,k-1,6-begin[k]-end[k]);
				ans+=f(end,k-1,6-begin[k]-end[k]);
				ans++;
			}
			printf("Case %d: %lld\n",cnt++,ans);
		}
		return 0;
	}
}

int main()
{
	return Acc::work();
}
```
### 思考：
##### 1.$f( )$函数的递归：
事实上，$f( )$函数是可以处理任何情况的，因为它递归了。每次只处理一个盘子的移动，不管其他盘子如何变化。
##### 2.使用递归分治
本问题虽然有“最小步”，但是我们处理过程中都用的是最优策略。
同时，决策性问题可以多通过递归来实现，将大问题转化为小问题。

#### 反思
##### 1.$f( )$函数的终点
如果$f( )$函数的终点是`if(i==1) return 1`，即：
> 把1号盘子移动到final上，需要1步。
> 
如果终点是`if(i==0) return 0`，即：

> 现在没有盘子了，不需要移动。

那么我们向回推，$i=0$的上一个情况是$i=1$，分两种情况：

 1. $p[i]=final$：不需要移动，返回0。但是如果按`if(i==1) return 1`，显然第二种是错误的。
 2. $p[i]\not=final$：需要移动一步，恰好是1+0，第一种和第二种都正确。
 故我们选用第一种。
 ##### 2.while( )的正确姿势
 比较
 ```cpp
 while(k>=1 && begin[k]==end[k]) k--;
 ```
 与
 ```cpp
 while(k>=1)
 {
 	if(begin[k]==end[k]) k--;
 }
 ```
 显然效果是不同的，正确实现第一个语句：
 ```cpp
 while(k>=1)
 {
 	if(begin[k]==end[k]) k--;
 	else{
 		break;
 	}
 }
 ```
##### 3.压代码的艺术
若要正确实现

> 两个串相等，则直接输出0；若不相等，则去掉从大到小尽可能多的相同数字。

我们通过
```cpp
while(k>=1 && begin[k]==end[k]) k--;
ll ans=0;
if(k>0){
	ans=f(begin,k-1,6-begin[k]-end[k]);
	ans+=f(end,k-1,6-begin[k]-end[k]);
	ans++;
}
printf(ans);
```
轻松简洁地实现了，通过一个最长不等长度k进行判断。
##### 4. $2^n$的正确计算
不要快速幂了！！！直接`1<<n`它不香吗。遇到以$2$为底的都用左移，包括除法。