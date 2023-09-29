---
abbrlink: ''
categories:
- - oi
date: '2023-09-29T21:55:09.178632+08:00'
tags:
- oi
- qbxt
title: 等差
updated: 2023-9-29T21:55:17.637+8:0
---
# 题目

你在网上闲逛的时候，发现有文章提到等差数列。

你知道求和是简单的，但你不禁好奇求其乘积会怎样？

所以你需要解决以下问题：给定一个等差数列，求他的各项乘积，你只需要输出其对 $1145141$ 取模的结果。

具体的，每组给定 $d,n,a$ 分别表示公差，长度，首项，你需要求出 $\prod_{i=0}^{n-1} (a+i\times d) \mod 1145141$。

注意，本题有多组测试数据。

对于所有数据，满足 $0\le d,a \le 10^9,1\le n\le 10^9,1\le T\le 10^4$。


# 题解

测试点中存在`d == 1`的情况（没放出来），所以考虑一下

首先，显然读入的时候$d, a$都可以先`%mod`

此时乘积可以表示为$\frac{(a+n-1)!}{(a-1)!}$。

考虑`d != 1`如果$n > mod(1145141)$的时候，直接输出`0`即可

否则可以将其转化成`d == 1`的情况，将每一项都除以d，结果乘上$d^n$

预处理出阶乘数组，计算即可

需要用到乘法逆元，费马小定理即可


# 代码

```cpp
#include<bits/stdc++.h>

using namespace std;
#define int long long
const int mod = 1145141;
int fact[mod * 2 + 10];
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
		x = x * 10 + ch - '0';
		ch = getchar();
	}

	return f * x;
}

int fast_pow(int a, int n)
{
	int ans = 1;
	while(n)
	{
		if(n & 1)
		{
			ans = ans * a % mod;
		}
		n >>= 1;
		a = 1ll * a * a % mod;
	}
	return ans;

}

int T, d, n, a;

signed main()
{
	freopen("sequence.in", "r", stdin);
        freopen("sequence.out", "w", stdout);
	fact[0] = 1;
	for(int i = 1;i <= mod * 2;i++)
	{
		fact[i] = 1ll * i * fact[i - 1] % mod;
	}
	T = read();

	while(T --)
	{
		d = read() % mod, n = read(), a = read() % mod;
		if(d == 0)
		{
			printf("%lld\n", fast_pow(a, n));
		}
		else if(n > mod)
		{
			printf("0\n");
		}
		else if(a == 0)
		{
			printf("0\n");
		}
		else
		{
			a = 1ll * a * fast_pow(d, mod - 2) % mod;
			printf("%lld\n", (1ll * fast_pow(d, n) % mod * fact[a + n - 1] % mod * fast_pow(fact[a - 1], mod - 2) % mod) % mod);
		}

	}
	return 0;
}
```
