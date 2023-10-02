---
abbrlink: ''
categories:
- - oi
date: '2023-10-02T20:22:17.158314+08:00'
tags:
- oi
- zhx
- qbxt
title: 大数翻倍法求（拓展）中国剩余定理
updated: 2023-10-2T22:49:4.395+8:0
---
# 暴力

暴力合并每个数和另一个数，算一下要加上多少才能符合。

看着时间复杂度很迷惑，实际上模板题跑的还是很快的。

不过可能又被卡的风险，据zhx说n越小越容易卡，不过顶多卡一个点（

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int n;

void Merge(int &b1, int &a1, int b2, int a2)
{
	if(a1 < a2) swap(b1, b2), swap(a1, a2);
	while(b1 % a2 != b2)
	{
		b1 += a1;
	}
	a1 = a1 / __gcd(a1, a2) * a2;
}
//a为模数，b为余数 
signed main()
{
	ios::sync_with_stdio(0), cin.tie(0);

	int bstar = 0, astar = 1;
	int b, a;
	cin >> n;
	for(int i = 1;i <= n;i++)
	{
		cin >> a >> b;
		b%= a;
		Merge(bstar, astar, b, a);
	}

	printf("%lld", bstar);
	return 0;
}
```
