---
abbrlink: ''
categories:
- - qbxt
date: '2023-10-03T23:20:08.305886+08:00'
tags:
- oi
- qbxt
title: 奇怪的等式
updated: 2023-10-3T23:20:11.731+8:0
---
# 题目

KK 有一个正整数序列 $a_1,a_2,\ldots,a_n$，以及一个正整数 $P$。KK 认为一个整数三元组 $(i,j,k)$ 是好的，当且仅当**同时满足**以下条件：

- $1 \le i < j < k \le n$；
- $P=a_i\times 2^{\lfloor\log_2 a_j\rfloor+\lfloor\log_2 a_k\rfloor+2}+a_j\times 2^{\lfloor\log_2 a_k\rfloor+1}+a_k$。

请你帮 KK 求出好的三元组数量。
对于 $100\%$ 的数据，$1\le T\le 10^3,1\le n\le 10^5,\sum n\le 10^6,1\le a_i < 2^{20},1\le P < 2^{60}$。

# 题解

首先考虑暴力求，$O(n^3)$，肯定不行，考虑优化

可以预处理出每个数的log下取整的2次幂，然后式子可以拆分成有$k$的因式和没有$k$的因式
，枚举$k$并顺便算出另一个因式的个数。可以用map存，时间复杂度$O(n^2)$，还是不行

观察式子，可以发现这个式子其实是把$a_i$，$a_j$，$a_k$三个数在二进制下拼起来，组成$P$，所以我们枚举$j$，并枚举$a_j$在$P$的哪个位置。枚举过程中顺便维护序列中$j$前后的数字个数，用来统计答案

# 问题

1.$a_k$不能有前导0！！！
2.当`1 << t`会爆`long long`的时候，要改成`1ll << t`！！！

# 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int T, n;
const int N = 1e5 + 10;
int a[N];

unordered_map<int, int> suf, pre;
int len[N], L, P, bit[62];

inline int getlen(int u)
{
	int ans = 0;
	while(u)
	{
		ans ++;
		u >>= 1;
	}
	return ans;
}

signed main()
{
	bit[0] = 1;
	for(int i = 1;i <= 60;i++)
		bit[i] = bit[i - 1] * 2; 
	ios::sync_with_stdio(0), cin.tie(0);
	cin >> T;
	while(T --)
	{
		pre.clear(), suf.clear();
		int ans = 0;
		cin >> n, cin >> P, L = getlen(P);
//		cout << "P LEN" << L << endl;
		for(int i = 1;i <= n;i++)
			cin >> a[i], len[i] = getlen(a[i]);

		for(int i = 2;i <= n;i++)
			suf[a[i]]++;
		for(int j = 2;j < n;j++)
		{
			int i = j;
			pre[a[j - 1]] ++;
			suf[a[j]] --;
			for(int start = 2; start + len[i] - 1 < L;start++)
			{
				int t = (L - start - len[i] + 1);
				if(((P >> t) & ((1 << len[i]) - 1)) != a[i] || !((P >> (t - 1)) & 1)) continue;
				int before = (P >> (L - start + 1)), after = (P & ((1ll << t) - 1));
//				if(j == 3)
//					cout << before <<" " << after<< endl;
				ans += pre[before] * suf[after];
			}
//			cout << ans << endl;
		}
		cout << ans << endl;
	}
}

/*
input:
1
5 7
8 8 1 1 1

std: 1
output: 0

*/ 
```

