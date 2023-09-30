---
abbrlink: ''
categories:
- - oi
date: '2023-09-30T20:58:27.132122+08:00'
tags:
- oi
- qbxt
title: 阶乘
updated: 2023-9-30T20:58:36.976+8:0
---
# 题目

给定两个正整数 $n$ 和 $m$，以及一个长为 $m$ 的序列 $a$。

请计算出最大的 $k$，使得能在序列中选出 $k$ 个数 $b_1,b_2,...,b_k$，满足 $b_1 ! \times b_2 ! \times \cdots \times b_k !$ 是 $n!$ 的因数。

对于 $100\%$ 的数据，$1 \leq m,a_i \leq 2 \times 10^5$，$1 \leq n \leq 10^9$。


# 题解

首先考虑暴力，发现会出现很多问题，包括超long long等问题，~所以考虑正解~

观察到一个性质，选小的集合一定比选大的集合更优，所以先把输入的$a$排个序，然后就可以二分$k$，check一下前$k$个是否是$n$的因数

~然后我考场上就卡在这里了~

考虑一下，如何判断一个大数是否是另一个大数的因数呢——分解质因数

不过对于这么大的数，分解并保存检验他的质因数显然不太合理，于是我们考虑先把所有的质数求出来再来计算——埃氏筛即可

对于我们筛出来的每一个质数，在$n!$和$b_1 ! \times b_2 ! \times \cdots \times b_k ! $的质因数分解后的集合中分别统计其出现了多少次，如前者小于后者则不可行。转化成下面两个问题

1.如何统计一个质数在$n!$的质因数分解集合中出现了几次

> $n!$中可能会出现$p$的倍数，$p^2$的倍数，$p^3$的倍数，如何计算呢？

> $n! = 1 *p * \cdots*2*p*\cdots*3*p*\cdots*(n/p)*p$

> 然后是$p^2$，由于其的一部分已经在$p$的时候算过了，所以我们只需让$n/=p$再次计算，以此类推，最后所有的结果相加即可

2.如何统计一个质数在$b_1 ! \times b_2 ! \times \cdots \times b_k !$中出现了几次

> 首先我们创立一个`cnt`数组，使之记录上式中每一个数出现了几次>

> 然后就和上面同理，看$p$的倍数，$p^2$的倍数，$p^3$的倍数。

另，注意到筛质数只需要处理到最大的$a$即可


# 代码

```cpp
#include<bits/stdc++.h>

using namespace std;
#define maxn 200000
#define ll long long
const int N = 2e5 + 10;
int a[N], p[N], vis[N], cnt[N], tot;
int n, m;

int ask(int p, int n)
{
	if(n == 0) return 0;
	return n / p + ask(p, n / p);
}

bool check(int x)
{
	memset(cnt, 0, sizeof cnt);
	for(int i = 1;i <= x;i++)
		cnt[a[i]]++;
	for(int i = N - 2;i >= 0;i--)
	{
		cnt[i] += cnt[i + 1];
	}

	for(int i = 1;i <= tot;i++)
	{
		int c = ask(p[i], n);
		long long b = 0;
		for(int j = p[i]; j < N / p[i];j *= p[i])
		{
			for(int k = j;k < N;k += j)
			{
				b += cnt[k];
			}
		}
//		cout << "qwq b:" << b << " c:" << c << endl;
		if(b > c) return 0;
	}
	return 1;
}

//bool check(int x){
//	memset(cnt,0,sizeof(cnt));
//	for (int i=1;i<=x;i++)cnt[a[i]]++;
//	for (int i=maxn;i>=1;i--)cnt[i]+=cnt[i+1];
//	for (int i=1;i<=tot;i++){
//		int c=ask(n,p[i]);
//		ll d=0;
//		for (int j=p[i];j<=maxn/p[i];j*=p[i]){
//			for (int k=j;k<=maxn;k+=j)
//				d+=cnt[k];
//		}
//		cout << "qwq b:" << d << " c:" << c << endl;
//		if (d>c)return 0;
//	}
//	return 1;
//}
int main()
{
    freopen("a.in", "r", stdin);
    freopen("a.out", "w", stdout);
	ios::sync_with_stdio(0), cin.tie(0);
	cin >> n >> m;
	for(int i = 1;i <= m;i++)
	{
		cin >> a[i];
	}
	sort(a + 1, a + 1 + m);
	vis[0] = vis[1] = 1;
	for(int i = 2;i < N;i++)
	{
		if(vis[i])
		{
			continue;
		}
		p[++tot] = i;
		for(int k = 2 * i;k < N;k += i)
		{
			vis[k] = 1;
		}
	}
	int l = 0, r = m;


//	cout << check(1);
	while(l < r)
	{
		int mid = (l + r + 1) >> 1;
		if(check(mid))
			l = mid;
		else
			r = mid - 1;
	}
	printf("%d", l);
}
```
