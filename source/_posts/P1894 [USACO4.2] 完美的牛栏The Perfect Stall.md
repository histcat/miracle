---
abbrlink: ''
categories:
- - oi
date: '2023-10-13T21:13:36.541789+08:00'
tags:
- oi
title: P1894
updated: 2023-10-13T21:13:37.552+8:0
---
# 题目

[link](https://www.luogu.com.cn/problem/P1894)

# 题解

由于每头奶牛只会和牛栏起关系，而让我们求最多能分配到多少牛栏，所以我们可以建立**二分图**

# 代码

```cpp
//二分图最大匹配
#include<bits/stdc++.h>

using namespace std;

const int N = 510;
int head[N], nxt[50010], to[50010], cnt = 1;
int n, m, e;

void add(int x, int y)
{
	to[++cnt] = y;
	nxt[cnt] = head[x];
	head[x] = cnt;
}

int vistime[N], match[N];

bool dfs(int u, int vist)
{
	if(vistime[u] == vist) return 0;//成环了就不能匹配了 
	vistime[u] = vist;

	for(int i = head[u]; i; i = nxt[i])
	{
		int v = to[i];
		if(match[v] == 0 || dfs(match[v], vist))
		{
			match[v] = u;
			return true;
		}
	}
	return 0;
}

int main()
{
	ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);

	cin >> n >> m;
	for(int i = 1, v, s;i <= n;i++)
	{
		cin >> s;
		for(int j = 1;j <= s;j++)
		{
			cin >> v;
			add(i, v);
		}
	}
	int ans = 0;
	for(int i = 1;i <= n;i++)
	{
		if(dfs(i, i))
		{
			ans ++;
		}
	}
	cout << ans;
	return 0;
}
```
