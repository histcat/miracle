---
abbrlink: ''
categories:
- oi
date: '2022-08-13 13:09:25'
mathjax: true
tags:
- oi
- 多重背包
title: 单调队列优化多重背包
updated: '2022-08-13 13:09:25'
---首先说一些写本文时的悲惨经历，一开始我是使用Gridea，编辑也是用它的编辑器，但是，在一次写作中，电脑无征兆地蓝屏了。我重启之后发现md文件打不开了，查看了一下二进制全是0 emmm（编写过程中保存了！！）。自此我换成了hexo emmm。

## 符号定义

首先我们定义一些符号，物品个数为 $n$ ，
第 $i$ 个物品的体积为 $v_i$ ，价值为 $w_i$ , 最多为 $s_i$ 个。
背包的体积为 $V$，动归数组为 $dp$ .

## 回顾

我们先回顾多重背包的优化，（ $x$ 是 背包体积为 $j$ 时，背包能够承受的最大个数，即 $j / v$）
设此时第 $i$ 个物品的体积为 $v$ .

```cpp
dp[i, j]= max(dp[i - 1, j], dp[i - 1, j - v] + w, dp[i - 1, j - 2v] + 2w, ......,dp[i - 1, j - (x - 1) v] + (x - 1)w, dp[i - 1, j - xv] + xw)
```

然后我们来思考，完全背包时，$dp[i,j]$ 和 $dp[i,j - v]$ 的联系

```cpp
dp[i, j - v]= max(dp[i - 1, j - v], dp[i - 1, j - 2v] + w, dp[i - 1, j - 3v] + 2w, ......, dp[i - 1, j - (x - 1) v] + (x - 2)w, dp[i - 1, j - xv] + (x - 1)w)
```

（有的同学可能会问最后一个背包能承受的数量为什么还是 $x$ ，但其实那不是 $x$ ，是 $ (x - 1)v - v $ 即 $xv$ ）

然后我们惊奇地发现， $max()$ 第一项之后的 和 $dp[i,j - v]$ 是如此的相似，只差了个 $w$ ，所以我们可以把 $w$移出来，替换成

```cpp
dp[i, j]= max(dp[i - 1, j], dp[i][j - v] + w)
```

## 否定

然后我们来看多重背包，我们还推柿子看看（我们先假设此时 $j$ 远大于 $sv$ ）

我们先尝试能否用完全背包的方法来优化，

```cpp
dp[i, j]= max(dp[i - 1, j], dp[i - 1, j - v] + w, dp[i - 1, j - 2v] + 2w, ......,dp[i - 1, j - (s - 1) v] + (s - 1)w, dp[i - 1, j - sv] + sw
```

```cpp
dp[i, j - v]= max(dp[i - 1, j - v], dp[i - 1, j - 2v] + w, dp[i - 1, j - 3v] + 2w, ......, dp[i - 1, j - (s - 1) v] + (s - 2)w, dp[i - 1, j - sv] + (s - 1)w)
```

看起来似乎很对，是吗？（

但是我们考虑一下项数，我们会发下 $dp[i, j-v]$ 的项数 和 $dp[i,j]$相同，都是 $s$ 项 ，不和 $max()$ 第一项之后的 项数相同，从而不能使用之前的方法优化

## 展新

（从这一小节开始用一维数组，方便表示，没学过一维数组表示的去学一下 ）

我们还是考虑 $dp[j]$，还是列柿子

```cpp
dp[j]= max(dp[j], dp[j - v] + w, dp[j - 2v] + 2w, ......,dp[j - (s - 1) v] + (s - 1)w, dp[j - sv] + sw
```

我们观察这个柿子，很容易得出一个性质，会更新状态 $j$ 的状态和 $j$ 都是模 $v$ 意义下**同余**的。于是我们把状态 $1~V$ 划分成 $v$ **类**，分别是模 $v$ 余 $0$ 的，模 $v$ 余 $1$ 的，$\cdots$，模 $v$ 余 $v-2$ 的，以及模 $v$ 余 $v-1$ 的。

原来的`dp[1]~dp[V]`我们就可以变化成这 $v$ 类（这里比较难想，但一定要想明白）

```cpp
dp[0]   dp[v]     dp[2v]     dp[3v]     ... dp[kv]
dp[1]   dp[1+v]   dp[1+2v]   dp[1+3v]   ... dp[1+kv]
dp[2]   dp[2+v]   dp[2+2v]   dp[2+3v]   ... dp[2+kv]
...
dp[v-1] dp[v-1+v] dp[v-1+2v] dp[v-1+3v] ... dp[v-1+kv]
```

然后我们就可以在这 $v$ 类**分别更新**了。（其中 $... + kv$ 为恰好满足最后一个状，看代码）

写一下这样的代码（先写一个二维的）（如果改为一维要倒序枚举状态或利用滚动数组）

```cpp
for(int i = 1;i <= n;i++)
{
    for(u = 0;u < v[i];u++)
    {
        for(int k = 0;u + k * v[i] <= V;k++)
        {
            int e = u + k * v[i];
            for(int t = min(k - s * v[i], u)/*防止减到负数*/;t < k;t += v[i])
            {
                dp[i][e] = max(dp[i - 1][e], dp[i - 1][t] + (e - t) / v[i] * w[i]);
            }
        }
	}
}
```

对，你没有看错，这是四层循环，也许你要问了，~~这循环怎么还越搞越多了~~，但是让我们分析一下他的时间复杂度。第一层循环 $O(n)$，第二层循环是枚举每一类的起始值的，第三层是枚举该类里每一个状态的，二三层的总共时间复杂度是 $O(V)$，最后一层就是枚举 可以转移到当前状态的状态，是 $O(s)$ 的，所以总共时间复杂度还是 $O(nVs)$ 的。

如果上面没明白我们举个例子。

举个例子吧，假设有 $2$ 件物品，$v_1 = 5$，$w_1 = 6$，$s_1 = 3$，$v_2 = 4$，$w_2 = 8$，$s_2 = 5$，背包体积为 $31$

在 $i = 1$ 即枚举第一个物品的时候，dp状态的分类为

```cpp
dp[0] dp[5] dp[10] dp[15] dp[20] dp[25] dp[30]
dp[1] dp[6] dp[11] dp[16] dp[21] dp[26] dp[31]
dp[2] dp[7] dp[12] dp[17] dp[22] dp[27]
dp[3] dp[8] dp[13] dp[18] dp[23] dp[28]
dp[4] dp[9] dp[14] dp[19] dp[24] dp[29]
```

```cpp
//第一（余数为0）的一类
dp[1][0]  = dp[0][0]
dp[1][5]  = max(dp[0][5], dp[0][0] + w[1])
dp[1][10] = max(dp[0][10], dp[0][5] + w[1], dp[0][0] + 2 * w[1])
dp[1][15] = max(dp[0][15], dp[0][10] + w[1], dp[0][5] + 2 * w[1], dp[0][0] + 3 * w[1])
dp[1][20] = max(dp[0][20], dp[0][15] + w[1], dp[0][10] + 2 * w[1], dp[0][5] + 3 * w[1])
//最多选3个，所以最多体积（第二维）最多减3个v[i]
dp[1][25] = max(dp[0][25], dp[0][20] + w[1], dp[0][15] + 2 * w[1], dp[0][10] + 3 * w[1])
dp[1][30] = max(dp[0][30], dp[0][25] + w[1], dp[0][20] + 2 * w[1], dp[0][15] + 3 * w[1])

    
//下同
```

$i = 2$ 也一样

## 单调队列优化

也许你要问了，~~说了这么多怎么连单调队列的影子都没看见~~，其实我们之前做的都是为了ta做铺垫，ta这不就来了。

首先，我们要优化的就是上面的代码的第四层：由当前状态的前面 $s$ 个（不相邻的，每两个相差 $v$）状态来更新当前状态。

我们从感性地角度来思考一下，上图！（qwq

![mul-p1](https://cdn.staticaly.com/gh/JesseJeson/picture-api@master/20220811/mul-p1.4f1ei1086w40.png)

我们来看这个图， $j$ 这里表示的就是当前状态 ，$j'$表示的是下一个状态 即 $j + v$，灰色格子表示的是  ***可以更新当前状态（这里为 $j$ 或 $j'$）**的状态*

**！！！！！注意：图片里第一行最左面的应该是 $j-sv$ 手贱写错了（懒得改了emmm）**

我们可以观察出来，**状态**每增加 $v$ ta的式子`dp[j]= max(dp[j], dp[j - v] + w, dp[j - 2v] + 2w, ......,dp[j - (s - 1) v] + (s - 1)w, dp[j - sv] + sw`就会失去最后（图上最左）面的一个  可以更新ta的状态  ，同时也得到了最前（图上最右）面的一个 可以更新ta 的状态。

让我们思考一下，可以在两端插入（因为 $j$ 每次增加 $v$，max函数每就会多一个可用来更新 当前状态$j$ 的状态），删除数据（也会少一个），并支持较小 （$O(1)$）的时间复杂度求最大值（为max函数要求）的数据结构是什么？**单调队列**！(单调队列里存的是下标，而不是具体数据)

## 一些小细节

### 偏移量

```cpp
dp[0]   dp[v]     dp[2v]     dp[3v]     ... dp[kv]
dp[1]   dp[1+v]   dp[1+2v]   dp[1+3v]   ... dp[1+kv]
dp[2]   dp[2+v]   dp[2+2v]   dp[2+3v]   ... dp[2+kv]
...
dp[v-1] dp[v-1+v] dp[v-1+2v] dp[v-1+3v] ... dp[v-1+kv]
```

我们把类的起点（指`dp[0]`到`dp[v-1]`），随便选一个，设为 $u$ 来看。

```cpp
dp[u]    = dp[u]
dp[u+v]  = max(dp[u] + w, dp[u + v])
dp[u+2v] = max(dp[u] + 2w, dp[u + v] + w, dp[u + 2v])
dp[u+3v] = max(dp[u] + 3w, dp[u + v] + 2w, dp[u + 2v] + w, dp[u + 3v])
...
```

我们发现在max函数里，dp[] 后面加的值不一样，这就很难受，我们在单调队列里更新值很不方便，所以我们搞点花样。

```cpp
dp[u]    = dp[u]
dp[u+v]  = max(dp[u], dp[u + v] - w) + w
dp[u+2v] = max(dp[u], dp[u + v] - w, dp[u + 2v] - 2w) + 2w
dp[u+3v] = max(dp[u], dp[u + v] - w, dp[u + 2v] - 2w, dp[u + 3v] -3w) + 3w
...
```

这样看起来就很舒服了。每次进单调队列的值就是`dp[u + kv] - k*w`。更新当前状态的时候最后别忘了加上 $a\\times w$ ，a就是 $（当前状态-队首状态）/v$。为什么呢，我们来看一看

我们知道，队首`dp[u + kv] - k*w`为 $u$ 固定，$k$ 在范围内任意取得的最大值。我们在更新当前状态(假设为 $u + bv$)的时候 ，max函数外的偏移量为 正的$bv$ ，而`dp[u + kv]`比`dp[u + kv] - k*w`少减了 $kw$ ，而最后max函数外又要给ta加上 $bv$ ，所以综合下来就是加上 $\(b - k\) * w$ 也就是  $（当前状态-队首状态）/v * w$

队列里个数应为 $s+1$ 个，为 $0$ 到 $s$ 个 前面状态

### 滚动数组

前面提到过，多重背包要么倒序枚举，要么滚动（备份）数据，但单调队列的性质来看，倒序枚举不太方便，所以我们采用备份数组的方式

## 代码

然后我们就可以~~愉快地~~敲代码了

```cpp
#include<iostream>
#include<cstdlib>
#include<cstring>//memcpy函数需要的头文件
using namespace std;
const int N = 200005;

int q[N];//单调队列，实际存的是下标
int g[N];//备份数组
int dp[N];

int main()
{
    int n, V;
    cin >> n >> V;
    for(int i = 1;i <= n;i++)
    {
        int v, w, s;//可以选择边读入边计算，就可以不用创建数组了
        memcpy(g, dp, sizeof dp);//备份
        cin >> v >> w >> s;
        for(int u = 0;u < v;u++)//枚举每一类的起点
        {
            int hh = 0, tt = -1;//因为单调队列这选择维护的是一个闭区间，如果tt也等于1
            //表示的就是有一个元素了qwq
            for(int k = u;k <= V;k += v)//这里的k含义和上面有所区别
            {
                while(hh <= tt && (k - q[hh]) > s * v) hh++; //超出s就弹出
                while(hh <= tt && g[q[tt]] - (q[tt] - u) / v * w < (g[k] - (k - u) / v * w)) tt--;//保持队列单调
                q[++tt] = k;//把当前状态放入
                if(hh <= tt) dp[k] = max(dp[k], g[q[hh]] + (k - q[hh]) / v * w);//更新当前状态
            }
        }
	}
    cout << dp[V];
    return 0;
}
```

完结撒花！

