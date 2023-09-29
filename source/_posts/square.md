---
abbrlink: ''
categories:
- - oi
date: '2023-09-29T21:47:52.038288+08:00'
tags:
- oi
- qbxt
title: 正方形
updated: 2023-9-29T21:48:0.692+8:0
---
# 题目

你有一个大小为 $n\times n$ 的矩阵，矩阵每个格子有一个颜色 $a_{i,j}\le n$。

你喜欢大正方形，但你不喜欢丰富的颜色，具体的，给定讨厌值 $k \le n$，你需要对于所有格子，计算出以这个格子为左上角的最大正方形，满足内部颜色种数不超过 $k$。

你需要对于每个格子输出最大正方形的边长 $len$。

注意，正方形不能超出边界。

对于所有数据，满足 $1\le n\le 500,1\le k \le n,1\le a_{i,j}\le n$。


# 题解

首先考虑暴力枚举 $n^5$，显然不可行

考虑优化

看到值域较小，于是乎枚举每一个元素，对于每个元素计算出`g[i][j]`代表这个元素`A`在`(i, j)`这个点扩展到长度为`len`时候正好被新加入正方形中

`g[i][j] = min(g[i + 1][j] + 1, g[i][j + 1] + 1, g[i + 1][j + 1] + 1);`

然后设`f[i][j][len]`表示当`i, j`这个点扩展到长度为len时会新加入多少点

在计算`g[i][j]`的时候顺便`f[i][j][g[i][j]]++`即可


# 代码

```cpp
#include<bits/stdc++.h>

using namespace std;

const int N = 510;
int a[N][N];

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

int n, k;

int g[N][N];//第A个数会在以(i, j)为左上角的正方形 边长为g[i][j] 时出现 
int f[N][N][N];

int main()
{
    freopen("square.in", "r", stdin);
    freopen("square.out", "w", stdout);
	n = read(), k = read();
	for(int i = 1;i <= n;i++)
	{
		for(int j = 1;j <= n;j++)
		{
			a[i][j] = read();
		}
	}

	for(int A = 1;A <= n;A++)
	{
		for(int i = n;i >= 1;i--)
		{
			for(int j = n;j >= 1;j--)
			{
				g[i][j] = 0x3f3f3f3f;
				if(i + 1 <= n) g[i][j] = min(g[i][j], g[i + 1][j] + 1);
				if(j + 1 <= n) g[i][j] = min(g[i][j], g[i][j + 1] + 1);
				if(i + 1 <= n && j + 1 <= n) g[i][j] = min(g[i][j], g[i + 1][j + 1] + 1);
				if(a[i][j] == A) g[i][j] = 1;
				if(g[i][j] <= n) f[i][j][g[i][j]]++; 
			}
		}
	}

	for(int i = 1;i <= n;i++)
	{
		for(int j = 1;j <= n;j++)
		{
			int len = 1, tot = f[i][j][1];//以i，j为左上角， 边长为len第一次出现了多少新颜色 
			while(i + len <= n && j + len <= n && f[i][j][len + 1] + tot <= k) len++, tot += f[i][j][len];
			printf("%d ", len);
		}
		printf("\n");
	}

	return 0;
}
```
