---
abbrlink: ''
categories:
- - oi
date: '2023-09-30T21:58:31.294420+08:00'
tags:
- oi
- qbxt
title: 合并
updated: 2023-9-30T21:58:41.458+8:0
---
# 题目

黑板上写着一行 $n$ 个数，小明每次可以选择连续的 $k$ 个数，将它们从黑板上擦去，并把它们的异或值写到黑板上它们原来所在的位置上。

小明会这样操作，直至黑板上只剩下一个数。请问在所有的使得黑板上最后只剩下一个数的操作方案中，小明写下数字之和的最小值是多少。

保证 $n-1$ 是 $k-1$ 的倍数，也即一定可以操作至只剩下一个数。

对于 $100\%$ 的数据，满足 $2 \leq n \leq 500$，$2 \leq k \leq 50$，$0 \leq a_i \leq 1023$。

# 题解

首先考虑`k==2`的特殊情况，发现为简单的区间dp

然后拓展到`k!=2`时，发现二维状态难以表示，于是乎加上一维，设`f[i][j][c]`表示`i--j`这个区间内，最终由**恰好**`c`个区间合成一个数字，所需的最少代价

考虑转移

$$
f_{i,j,c}=min^{r-1}_{j=l} f_{l,j,1}+f_{j+1,r,c-1}
$$

虽然状态中的`c`只分割到1和c-1，但是其包含了剩余的情况

但是是$O(n^3k)$的时间复杂度，过不了，考虑去除冗杂的状态

合法的区间必须满足$(k-1)|(r-l+1-c)$，这样才能够保证其能最终合成一个数字

于是时间复杂度便成了$O(\frac{n^3}{k})$，可以过

# 代码

```cpp
#include<bits/stdc++.h>

using namespace std;

const int N = 505;
int a[N], xsum[N];
int n, k;
int f[N][N][N];//代表从i到j由c个区间合成，所能导致的最小代价 

int dfs(int l, int r, int c)
{
	if(l == r)
	{
		if(c == 1) return 0;
		else return 0x3f3f3f3f;
	}
	if(c == 1) c = k;
	if(f[l][r][c] != -1) return f[l][r][c];

	int ans = 0x3f3f3f3f;
	for(int i = l;i < r;i += (k - 1)) ans = min(ans, dfs(l, i, 1) + dfs(i + 1, r, c - 1));
	if(c == k) ans += xsum[r] ^ xsum[l - 1];
	return f[l][r][c] = ans;
}

int main()
{
    freopen("b.in", "r", stdin);
    freopen("b.out", "w", stdout);
	memset(f, -1, sizeof f); 
	scanf("%d%d", &n, &k);
	for(int i = 1;i <= n;i++) cin >> a[i], xsum[i] = xsum[i - 1] ^ a[i];

	printf("%d", dfs(1, n, 1));
	return 0;
}
```



