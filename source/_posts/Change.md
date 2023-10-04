---
abbrlink: ''
categories:
- - oi
date: '2023-10-04T22:28:34.022904+08:00'
tags:
- oi
- qbxt
- dtoj
title: 修改
updated: 2023-10-4T22:28:34.983+8:0
---
# 题目

给定一个长度为 $n$ 的序列，第 $i$ 个元素的下标为 $i$，值为 $a_i$

现有 $q$ 次操作，每次给定一个区间 $[l,r]$，求该区间的元素和，并将区间内的所有元素值修改为 $0$

需要你输出所有操作答案的异或和

由于 $n, q$ 很大，我们提供另外一种读入方式

给定一个 $z$，注意其类型为 `unsigned int`，你需要调用 $n$ 次 $gen()$ 函数获得序列，再调用 $2n$ 次获得每次操作的 $l,r$，具体代码如下：

```c++
const int N = 1e9;
const int maxn = 1e7 + 10;
unsigned int x = 123456789, y = 362436069, z, a[maxn];
int n, q;
unsigned int gen()
{
    unsigned int t;
    x ^= x << 16; x ^= x >> 5; x ^= x << 1;
    t = x; x = y; y = z; z = t ^ x ^ y;
    return z % N + 1;
}

int main()
{
    scanf("%d%d%u", &n, &q, &z);
    for ( int i = 1; i <= n; ++ i ) 
        a[i] = gen();
    for ( int i = 1; i <= q; ++ i ) 
    {
        int l = gen() % n + 1, r = gen() % n + 1;
        if ( l > r ) swap(l, r);
        // do your things ...
    }
}

```


对于 $40\%$ 的数据，满足 $n, q\le 10^6$

对于 $100\%$ 的数据，满足 $n, q\leq 10^7$

# 题解

~~我才不会告诉你考场上我想了1个小时还没想出来~~

开始想到线段树，看数据范围，妥妥超时

~~然后就不知道怎么做了~~

既然查询完接着删除，那么一个数查询完之后接着就可以不用管了

考虑用并查集维护不用管的数，开始的时候开$n$个并查集树

查询的时候，顺便维护一下（把$l$到$r$的fa都指向`fa[r + 1]`）

（其实用链表维护也可以过）

~~我好菜~~


# 代码

```cpp
#include<bits/stdc++.h>
#define ll long long

using namespace std; 

const int N = 1e9;
const int maxn = 1e7 + 10;
unsigned int x = 123456789, y = 362436069, z, a[maxn];
int n, q;
int fa[maxn]; 
unsigned int gen()
{
    unsigned int t;
    x ^= x << 16; x ^= x >> 5; x ^= x << 1;
    t = x; x = y; y = z; z = t ^ x ^ y;
    return z % N + 1;
}

int getfa(int u)
{
	if(fa[u] == u) return u;
	return fa[u] = getfa(fa[u]);
}

void merge(int x, int y)
{
	int xfa = getfa(x), yfa = getfa(y);
	fa[xfa] = yfa;
}

int main()
{
    scanf("%d%d%u", &n, &q, &z);
    for (int i = 1;i <= n;i++) 
        a[i] = gen(), fa[i] = i;
    fa[n + 1] = n + 1;
    unsigned ll ans = 0;
    for (int i = 1;i <= q;i++) 
    {
        int l = gen() % n + 1, r = gen() % n + 1;
        if (l > r) swap(l, r);
        unsigned ll b = 0;
        int g;
		for(int i = getfa(l);i <= r;i = g)
		{
			b += a[i];
			g = getfa(i + 1);
			merge(i, i + 1);
		}
		ans ^= b;
	
    }
    cout << ans;
}
```
