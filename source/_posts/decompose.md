---
abbrlink: ''
categories:
- oi
date: '2022-08-09 16:13:59'
mathjax: true
tags:
- oi
title: 浅谈分块
updated: '2022-08-12 18:52:46'
---## 分块含义

> 分块是一种思想，而不是一种数据结构。
> 分块的基本思想是，通过对原数据的适当划分，并在划分后的每一个块上预处理部分信息，从而较一般的暴力算法取得更优的时间复杂度。

## 时间复杂度

> 分块的时间复杂度主要取决于分块的块长，一般可以通过均值不等式求出某个问题下的最优块长，以及相应的时间复杂度。

### 均衡不等式

对于我们初中所学的完全平方公式 $(x+y)^2 \ge 0$ 经过简单的变形可以得到 $x^2+y^2 \ge 2xy$

我们把 $x = \sqrt{a}$ ， $y = \sqrt{b}$ 带入可以得到 $a + b \ge 2\sqrt{ab}$ $(a, b > 0)$ ，在 $a = b$ 时取等号，这就是均衡不等式

### 分析

以一道题目为例：[线段树1](https://www.luogu.com.cn/problem/P3372)

~~没错，虽然是线段树的模板题，但也可以用分块来做，而且跑的贼快~~

我们设块长为 $s$ ，无论什么\[l, r\]操作都是

1. $l, r$ 在同一块中，枚举： $O(s)$
2. $l, r$ 不在同一块中：枚举 $l$ 所在块，枚举 $l$ 所在块的下一个 到 $r$ 所在块的上一个，枚举 $r$ 所在块： $O(s + \frac{n}{s})$

共有 $m$ 次操作的话，总时间复杂度就是 $O(m(s + \frac{n}{s}))$ ，利用均衡不等式我们可知
当 $s = \frac{n}{s}$ 时该式子取得值最小，即时间复杂度最优，此时 $s = \sqrt{n}$

（tips：对于不同的题目可能块长 $s$ 不一样，要具体分析，但一般来说类似此题这种操作的块长取得都是 $\sqrt{n}$）

## 模板

对于刚刚的那道线段树题目，我们可以写出分块的代码

```cpp
#include <bits/stdc++.h>

using namespace std;
const int N = 1e5 + 5;

int n, m;
long long a[N], blk[N], b[N];
int pos[N];
int R;

void add(int l, int r, long long k)
{
    if(pos[l] == pos[r])
    {
        for(int i = l;i <= r;i++)
            a[i] += k;
        blk[pos[l]] += (r - l + 1) * k;
        return;
    }
    for(int i = l;pos[i] == pos[l];i++)
    {
        a[i] += k;
        blk[pos[i]] += k;
    }
    for(int i = pos[l] + 1;i <= pos[r] - 1;i++)
    {
        blk[i] += R * k;
        b[i] += k;
    }
    for(int i = r;pos[i] == pos[r];i--)
    {
        a[i] += k;
        blk[pos[i]] += k;
    }
}

long long query(int l, int r)
{
    long long ans = 0;
    if(pos[l] == pos[r])
    {
        for(int i = l;i <= r;i++)
        {
            ans += a[i] + b[pos[i]];
        }
        return ans;
    }
    for(int i = l;pos[i] == pos[l];i++)
    {
        ans += a[i] + b[pos[i]];
    }
    for(int i = pos[l] + 1;i <= pos[r] - 1;i++)
    {
        ans += blk[i];
    }
    for(int i = r;pos[i] == pos[r];i--)
    {
        ans += a[i] + b[pos[i]];
    }
    return ans;
}

int main()
{
    clock_t c1 = clock();
#ifdef LOCAL
    freopen("in.in", "r", stdin);
    freopen("out.out", "w", stdout);
#endif
    //------------------------------
    scanf("%d%d", &n, &m);
    R = sqrt(n) + 1;

    for(int i = 1;i <= n;i++)
    {
        scanf("%lld", &a[i]);
        pos[i] = i / R + 1;
        blk[pos[i]] += a[i];
    }
    long long opt, x, y, z;
    while(m--)
    {
        scanf("%lld", &opt);
        if(opt == 1)
        {
            scanf("%lld%lld%lld", &x, &y, &z);
            add(x, y, z);
        }
        else if(opt == 2)
        {
            scanf("%lld%lld", &x, &y);
            printf("%lld\n", query(x, y));
        }
    }

  
  
    //------------------------------
end:
    cerr << "Time Used:" << clock() - c1 << "ms" << endl;
    return 0;
}

tips:

1. $pos$ 数组记录的是每个元素所在的块的编号
2. $blk$ 数组记录的是每个块内实际的和
3. $b$ 数组像是线段树里的 $lazytag$ 记录的是已经加到 $blk$ 数组里的，但还没有“下传”到原数组里的值（但是这里不用下传也可以，代码里就没有下传）

## 一些题目

### 单点修改+查询区间内小于（等于）或大于（等于）某数的个数

to be continued....

```

```

```
