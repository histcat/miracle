---
abbrlink: ""
categories:
  - - oi
date: 2023-10-01T17:37:14.096583+08:00
tags:
  - oi
  - qbxt
title: Mexor
updated: 2023-10-1T17:37:15.737+8:0
---
# 题目

给定若干个自然数 $a_{1\sim n}$。

你需要选出其中一些数，然后将你选出的数划分为若干个集合。

你需要最大化每个集合 $\tt mex$ 的异或和，输出这个值。

一个集合的 ${\tt mex}$ 是指最小的不在这个集合中的自然数，例如 $\{0,1,2,4,5\}$ 的 ${\tt mex}$ 是 $3$，$\{1,2,3\}$ 的 ${\tt mex}$ 是 $0$。

保证 $1\le n\le 10^6$，$0\le a_i\le n$。


# 题解

首先可以判断出，如果一个集合的$\text{mex}$为$x$的话，那么这个集合一定要包含$1$到$x-1$，而对于`x`后面的数要不要都无所谓

所以我们可以先取出最大的$\text{mex}$，即最长的连续数+1

然后，考虑剩下的数。如果$y$能凑出来那么对于$z < y$，$z$一定能够凑出，所以想让$z \text{ }xor\text{ } ans$最大，从大向小考虑，如果$z \& ans=0$，那么就选出$1$到$z-1$。

~~考场上就**与**没想出来，`-30points`QAQ~~

# 代码

实现的不好，应该是$O(n^2)$的，实际可以做到$O(n)$（但由于数据太水n方能过去）

```cpp
#include<bits/stdc++.h>

using namespace std;

int read()
{
	int f = 1, x = 0;
	char ch = getchar();
	while(ch < '0' || ch > '9')
	{
		if(ch == '-') f = -1;
		ch = getchar();
	}

	while(ch >= '0' && ch <= '9')
	{
		x = 10 * x + ch - '0';
		ch = getchar();
	}
	return f * x;
}

const int N = 1e6 + 10;
int a[N], n;
int cnt[N];
int maxa = -1;

long long ans;
int main()
{
	freopen("mexor.in", "r", stdin);
	freopen("mexor.out", "w", stdout);
	n = read();

	for(int i = 1;i <= n;i++)
	{
		a[i] = read();
		cnt[a[i]]++;
		maxa = max(maxa, a[i]);
	}
	while(1)
	{
		int i = -1;
		for(;i <= maxa;i++)
		{
			if(cnt[i + 1] == 0)
			{
				break;
			}
		}
		if(i == -1) break;
		if((ans & (i + 1)) == 0)
		{
			ans += i + 1;
			for(int j = 0;j <= maxa;j++)
			{
				cnt[j] --;
			}
		}
		else
		{
			cnt[i] = 0;
		}
	}

	printf("%lld", ans);

	return 0;
}
```
