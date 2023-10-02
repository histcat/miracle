---
abbrlink: ''
categories:
- - oi
date: '2023-10-02T22:38:46.631240+08:00'
tags:
- oi
- qbxt
title: 小葱拿糖
updated: 2023-10-2T22:38:48.186+8:0
---
# 题目

小葱将买来的糖放进了冰箱冷藏，但是小葱想吃糖了，小葱希望把自己想吃的糖从冰箱里面拿出来。具体来说，在一张$N$行$N$列的方格图中，有若干颗糖，每颗糖都是横向或者竖向摆放的。对于一个横向摆放的糖，我们只能左右移动这颗糖（任意距离，但不能跨越其他糖），而对于一个竖向摆放的糖只能上下移动。我们的目标是把编号为$1$的糖从冰箱移出去，而移出去的方法是让该糖果能够移动到方格图的右边界的位置（只有一号糖果能移出去，其他糖即使在边界也不能出去）。我们保证任何一颗糖果长度至少为$2$，并且$1$号糖果一定横向放置。在糖果移动过程中不能穿越其他糖果，问在上述限制条件下小葱最少要移动多少次才能把$1$号糖果移出冰箱。如果$1$号糖果一开始就在右边界上请输出$0$。

对于$100\%$的数据，$1\leq N\leq8$，最多有$13$种不同的糖果，答案不超过$10$步。

# 题解

*搜索！*搜索！**搜索！**

每次移动任意糖任意步，把每一个状态存下来（为了判断是否重复），然后bfs

如何存呢？其实一个糖只需要记录它的一个坐标位置，另一个是不变的。

对于这些糖的坐标，可以用一个$n$进制数来存，最多有$13$ 种不同的糖果，所以$n^{13} - 1$
就够了，算一下，unsigned long long够了

# 注意

每次放入一个新的状态前**先**检测要放入的这个状态是不是答案，否则有可能会被卡超时（因为这个调了2个小时QAQ）

# 代码

```cpp
#include<bits/stdc++.h>
#define ull unsigned long long 
#define DOWN 1
#define RIGHT 0

using namespace std;

const int N = 10;
int a[N][N], n, typenum;
const int M = 15;
// map压ull 
map<ull, int> step;
queue<ull> qwq;
int towards[M], pos[M], len[M], another_pos[M];

void trans(ull state)
{
	int cnt = typenum;
	while(cnt >= 1)
	{
		ull b = state % n;
		pos[cnt] = b;
		state /= n;
		if(towards[cnt] == DOWN)
		{
//			cout << cnt << "点 DOWN" << endl;
			for(int i = b;i <= b + len[cnt] - 1;i++)
			{
				a[i][another_pos[cnt]] = cnt;
			}
		}
		else
		{
//			cout << cnt << "点 RIGHT" << endl;
			for(int i = b;i <= b + len[cnt] - 1;i++)
			{
				a[another_pos[cnt]][i] = cnt;
			}
		}
		cnt --;
	}
}

ull pack()
{
	ull start = 0;
	for(int i = 1;i <= typenum;i++)
	{
		start = start * n + pos[i];
	}
	return start;
}

int main()
{
	freopen("take.in", "r", stdin);
        freopen("take.out", "w", stdout);
	ios::sync_with_stdio(0), cin.tie(0);
	cin >> n;
	for(int i = 1;i <= n;i++)
	{
		for(int j = 1;j <= n;j++)
		{
			cin >> a[i][j];
		}
	}
	
	for(int i = 1;i <= n;i++)
	{
		for(int j = 1;j <= n;j++)
		{
			if(a[i][j] == 0) continue;
			int cl = a[i][j];
			if(a[i + 1][j] == cl) towards[cl] = DOWN;
			else towards[cl] = RIGHT;
			
			if(towards[cl] == DOWN) pos[cl] = i, another_pos[cl] = j;
			else pos[cl] = j, another_pos[cl] = i;
			
			int x = i, y = j;
			len[cl] = 0;
			while(x <= n && y <= n && a[x][y] == cl)
			{
				a[x][y] = 0;
				(towards[cl] == DOWN) ? (x ++) : (y++) ;
				len[cl] ++;
			}
			typenum = max(typenum, cl);
		}
	}
	ull start = 0;
	for(int i = 1;i <= typenum;i++)
	{
		start = start * n + pos[i];
	}
	step[start] = 0;
	qwq.push(start);
	
	while(qwq.size())
	{
		ull t = qwq.front();
		qwq.pop();
		memset(a, 0, sizeof a);
		trans(t);
		if(pos[1] + len[1] - 1 >= n)
		{
			cout << step[t] << endl;
			exit(0);
		}
		for(int i = 1;i <= typenum;i++)
		{
			if(towards[i] == DOWN)
			{
				for(int movement = 1;movement <= n;movement++)
				{
					if(a[pos[i] + len[i] - 1 + movement][another_pos[i]] != 0 || pos[i] + len[i] - 1 + movement > n) break;
					pos[i] = pos[i] + movement;
					ull now = pack();
					pos[i] -= movement;
					if(step[now] != 0)
						continue;
					qwq.push(now), step[now] = step[t] + 1;
					/*就这里*/if(i == 1 && pos[i] + len[i] - 1 + movement == n)
					{
						cout << step[now];
						exit(0);
					}
				}
				
				for(int movement = -1;movement >= -n;movement--)
				{
					if(a[pos[i] + movement][another_pos[i]] != 0 || pos[i] + movement < 1) break;
					pos[i] = pos[i] + movement;
					ull now = pack();
					pos[i] -= movement;
					if(step[now] != 0)
						continue;
					qwq.push(now), step[now] = step[t] + 1;
					if(i == 1 && pos[i] + len[i] - 1 + movement == n)
					{
						cout << step[now];
						exit(0);
					}
				}
			}
			else
			{
				for(int movement = 1;movement <= n;movement++)
				{
					if(a[another_pos[i]][pos[i] + len[i] - 1 + movement] != 0 || pos[i] + len[i] - 1 + movement > n) break;
					pos[i] = pos[i] + movement;
					ull now = pack();
					pos[i] -= movement;
					if(step[now] != 0)
						continue;
					qwq.push(now), step[now] = step[t] + 1;
					if(i == 1 && pos[i] + len[i] - 1 + movement == n)
					{
						cout << step[now];
						exit(0);
					}
				}
				
				for(int movement = -1;movement >= -n;movement--)
				{
					if(a[another_pos[i]][pos[i] + movement] != 0 || pos[i] + movement < 1) break;
					pos[i] = pos[i] + movement;
					ull now = pack();
					pos[i] -= movement;
					if(step[now] != 0)
						continue;
					qwq.push(now), step[now] = step[t] + 1;
					if(i == 1 && pos[i] + len[i] - 1 + movement == n)
					{
						cout << step[now];
						exit(0);
					}
				}
			}
		}
		//step等由这个点向外扩展的时候再更新 
	}
	
	return 0; 
}
```

