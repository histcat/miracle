---
abbrlink: ''
categories:
- - oi
date: '2023-10-01T22:32:54.859199+08:00'
tags:
- oi
- qbxt
title: Wbtree
updated: 2023-10-1T22:32:57.530+8:0
---
# 题目

给定一棵有根树，树上的每个节点是黑色或白色的。$1$ 号点是根。

请对于每个白色的点，在子树中找一个黑色的点与其匹配，其中每个黑点只能和一个白点匹配。你需要求出所有白点与其配对的黑点的距离之和最小是多少。

树上两点的距离定义为他们之间简单路径上的边数。

数据保证有解。

$1 < n < 10^6$

# 题解

首先可以考虑到贪心

如果以白点为主体，可以从深到浅枚举白点（因为深处的白点可选的黑点更少），然后对于每个白点维护子树里面里它最近的黑点，配对即可。

不过这么做似乎要可并堆~~我不会，todo~~

所以考虑以黑点为主体，可以从浅到深枚举黑点（因为浅处的黑点选择更少），对于每个黑点找到**它到根节点路径上的最深的，且未被配对的白点**，具体可以用并查集维护，看代码

# 代码

```cpp
#include<bits/stdc++.h>

using namespace std;

//----------------------------------------
const int N = 1e6 + 10;
int n;
bool color[N];
int fa[N], depth[N];
int head[N], nxt[N << 1], to[N << 1], cnt;
int vis[N];
long long white_dis_sum;
int vis_of_p[N];
void add(int x, int y)
{
    to[++cnt] = y;
    nxt[cnt] = head[x];
    head[x] = cnt;
}

void dfs(int u, int anc, int first_white)
{
    depth[u] = depth[anc] + 1;
    fa[u] = first_white;
    if(color[u] == 0) white_dis_sum += depth[u];
    for(int i = head[u]; i; i = nxt[i])
    {
        int v = to[i];
        if(v == anc) continue;
        if(color[u]/*black*/)
        {
            dfs(v, u, first_white);
        }
        else/*white*/
        {
            dfs(v, u, u);
        }
    }
}

int findfa_with_vis_equals_to_0(int u)
{
//	cout << "qaq" << u << endl;
    if(u == -1 || u == 0x3f3f3f3f) return 0x3f3f3f3f;
    if(vis[u] == 0) return u;
    return fa[u] = findfa_with_vis_equals_to_0(fa[u]);
}

int main()
{
    // for(int i = 1;i <= n;i++)
    // {
    //     fa[i] = i;
    // }
//    clock_t c1 = clock();
// #ifdef LOCAL
    freopen("wbtree.in", "r", stdin);
    freopen("wbtree.out", "w", stdout);
// #endif
//----------------------------------------
    ios::sync_with_stdio(0), cin.tie(0);
    cin >> n;
    for(int i = 1;i <= n;i++)
        cin >> color[i];
    int u, v;
    for(int i = 1;i <= n - 1;i++)
    {
        cin >> u >> v;
        add(u, v), add(v, u);
    }
    //找不到就是-1
    dfs(1, 0, -1);

    queue <int> qwq;
    qwq.push(1);
    long long ans = 0;
//    for(int i = 1;i <= n;i++)
//    {
//    	cout << i << ": " << fa[i] << endl;
//	}
    while(qwq.size())
    {
        int t = qwq.front();
//    	cout << "qwq"<<t << endl; 
		vis_of_p[t] = 1;
        for(int i = head[t]; i; i = nxt[i])
        {
            int v = to[i];
            if(vis_of_p[v]) continue;
            qwq.push(v);
        }
        qwq.pop();
        if(color[t] == 0) continue;
        int deepest_white = findfa_with_vis_equals_to_0(fa[t]);
//        cout << t << "的deepest_white: " << deepest_white << endl; 
        if(deepest_white == 0x3f3f3f3f) continue;
        vis[deepest_white] = 1;
        ans += depth[t];
    }
    cout <<  ans - white_dis_sum;
//----------------------------------------
//end:
//    cerr << "Time Used:" << clock() - c1 << "ms" << endl;
    return 0;
}

/*
 * ┌───┐   ┌───┬───┬───┬───┐ ┌───┬───┬───┬───┐ ┌───┬───┬───┬───┐ ┌───┬───┬───┐
 * │Esc│   │ F1│ F2│ F3│ F4│ │ F5│ F6│ F7│ F8│ │ F9│F10│F11│F12│ │P/S│S L│P/B│  ┌┐    ┌┐    ┌┐
 * └───┘   └───┴───┴───┴───┘ └───┴───┴───┴───┘ └───┴───┴───┴───┘ └───┴───┴───┘  └┘    └┘    └┘
 * ┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───────┐ ┌───┬───┬───┐ ┌───┬───┬───┬───┐
 * │~ `│! 1│@ 2│# 3│$ 4│% 5│^ 6│& 7│* 8│( 9│) 0│_ -│+ =│ BacSp │ │Ins│Hom│PUp│ │Num│ / │ * │ - │
 * ├───┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─────┤ ├───┼───┼───┤ ├───┼───┼───┼───┤
 * │ Tab │ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │{ [│} ]│ | \ │ │Del│End│PDn│ │ 7 │ 8 │ 9 │   │
 * ├─────┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴─────┤ └───┴───┴───┘ ├───┼───┼───┤ + │
 * │ Caps │ A │ S │ D │ F │ G │ H │ J │ K │ L │: ;│" '│ Enter  │               │ 4 │ 5 │ 6 │   │
 * ├──────┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴────────┤     ┌───┐     ├───┼───┼───┼───┤
 * │ Shift  │ Z │ X │ C │ V │ B │ N │ M │< ,│> .│? /│  Shift   │     │ ↑ │     │ 1 │ 2 │ 3 │   │
 * ├─────┬──┴─┬─┴──┬┴───┴───┴───┴───┴───┴──┬┴───┼───┴┬────┬────┤ ┌───┼───┼───┐ ├───┴───┼───┤ E││
 * │ Ctrl│ Win│ Alt│         Space         │ Alt│ Win│Menu│Ctrl│ │ ← │ ↓ │ → │ │   0   │ . │←─┘│
 * └─────┴────┴────┴───────────────────────┴────┴────┴────┴────┘ └───┴───┴───┘ └───────┴───┴───┘
 */
```

