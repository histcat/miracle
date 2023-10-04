---
abbrlink: ''
categories:
- - oi
date: '2023-10-04T22:33:57.960911+08:00'
tags:
- oi
- qbxt
- 组合数学
- 计数
title: title
updated: 2023-10-4T22:33:59.884+8:0
---
# 题目

给定一棵 $n$ 个点的以 $1$ 为根的有根树，现在有 $m$ 种颜色，你需要对每个节点染色

求本质不同的染色数，对 $998244353$ 取模

两棵树本质相同，当且仅当忽略节点编号后(根不变)，两棵树同构(颜色＋形态)

满足 $n≤500$

# 题解

首先，这是个计数问题，不擅长

计数方面的问题等学会了再来补充

然后看一下这个题目另一个关键点——如何确定一颗子树是不是同构？

> 树哈希

就是利用子树的某些性质，为这颗子树求出特定的值

可以利用不同的方法多次hash，以防被卡

# 代码

```cpp
#include<bits/stdc++.h>
#define int long long
const int mod = 998244353;
const int N = 510;
using namespace std;
int n, m;

int head[N], nxt[N << 1], to[N << 1], cnt = 1;

int siz[N];
unsigned long long hsh[N];
int sum[N];

void add(int x, int y)
{
	to[++cnt] = y;
	nxt[cnt] = head[x];
	head[x] = cnt;
}

void dfs1(int u, int fa)
{
//
	for(int i = head[u]; i; i = nxt[i])
	{
		int v = to[i];
		if(v == fa) continue;
		dfs1(v, u);
		siz[u] += siz[v];
	}
//	cout << " " <<u << " " << siz[u] << endl;
}

void dfs2(int u, int fa)
{
	hsh[u] = siz[u] + 10;
	for(int i = head[u]; i; i = nxt[i])
	{
		int v = to[i];
		if(v == fa) continue;
		dfs2(v, u);
		hsh[u] *= ((hsh[v] * 114 + 4869) * (hsh[v] * 114 + 4869) - 100);
	}
	hsh[u] -= 4869;
//	cout << " " <<u << " " << siz[u] << endl;
}

int ksm(int a, int n)
{
	if(a == 1) return 1;
	int ans = 1;
	while(n)
	{
		if(n & 1) ans = ans * a % mod;
		a = a * a % mod;
		n >>= 1;
	}
	return ans;
}

void dfs3(int u, int fa)
{
	sum[u] = m;
	unordered_map<unsigned long long, int> cnt;
	for(int i = head[u]; i; i = nxt[i])
	{
		int v = to[i];
		if(v == fa) continue;
		dfs3(v, u);
	}
	for(int i = head[u]; i; i = nxt[i])
	{
		int v = to[i];
		if(v == fa) continue;
		sum[u] = (sum[u] * (sum[v] + cnt[hsh[v]]) % mod) * ksm(cnt[hsh[v]] + 1, mod - 2) % mod;
		cnt[hsh[v]] ++;
	}
//	cout << u << " "  << sum[u] << endl; 
}

signed main()
{

	scanf("%lld%lld", &n, &m);
	for(int i = 1;i <= n;i++)
		siz[i] = 1;
	int u, v;
	for(int i = 1;i <= n - 1;i++)
	{
		cin >> u >> v;
//		cout << "qwq" << u << " " << v << endl;
		add(u, v), add(v, u);
	}
	dfs1(1, 0);
	dfs2(1, 0);
	dfs3(1, 0);

	printf("%lld\n", sum[1]);
	return 0;
}
```
