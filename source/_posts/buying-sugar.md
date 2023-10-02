---
abbrlink: ''
categories:
- - oi
date: '2023-10-02T13:07:54.365232+08:00'
tags:
- oi
- qbxt
title: 小葱买糖
updated: 2023-10-2T13:8:8.522+8:0
---
# 题目

小葱同学喜欢吃糖，小葱买了很多糖。但是小葱买的糖太多了，小葱记不清具体数字了，小葱只记得自己的总糖数是自己记录在笔记本上的$N$个数$a_1,a_2,\cdots,a_N$的最小公倍数。请你帮帮小葱，算算小葱买了多少糖。

对于$ 100\% $ 的数据，$ 1\leq N\leq 10^3,1\leq a_i\leq 10^9 $。


# 题解

提炼一下题意——求$ n $个数的最小公倍数

直接分解质因数，然后求出每个质因子的最大次幂是多少，乘起来即可

（第一次在考场上A题，不过是道签到题QAQ）


# 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

map <long long, long long>s;

const int N = 1e3 + 10;
int a[N], n;
const int mod = 1e9 + 7;
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

int fast_power(int a, int p)
{
	int ans = 1;
	while(p)
	{
		if(p & 1) ans = ans * a % mod;
		a = a * a % mod;
		p >>= 1;
	}
	return ans % mod;
}

signed main()
{
	freopen("buy.in", "r", stdin);
	freopen("buy.out", "w", stdout);
	int ans = 1;
	n = read();
	for(int i = 1;i <= n;i++)
		a[i] = read();
	int temp;
	for(int i = 1;i <= n;i++)
	{
		temp = a[i];
		for(int j = 2;j <= temp / j;j++)
		{
			int c = 0;
			while(temp % j == 0)
			{
//				cout << "qwq" << a[i] << " " << j << endl;
				c++;
				temp /= j;
				if(c > s[j])
				{
					ans = (ans * j) % mod;
					s[j]++;
				}
			}
//			if(c > s[j])
//			{
////				cout << "ans *=" << c - s[j] << "*"<< j <<endl;
//				ans = (ans * (c - s[j]) * j) % mod;
//				s[j] = c;
//			}
		}
		if(temp > 1)
		{
//			cout << "qwq" << a[i] << " " << temp << endl;
			if(s[temp] == 0)
			{
				s[temp] = 1;
//				cout << "ans *= " << temp <<endl;
				ans = ans * temp % mod;
			}
		}
	}
	printf("%lld", ans % mod);
	return 0;
}
```
