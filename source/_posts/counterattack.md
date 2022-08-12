---
abbrlink: ''
categories: []
date: '2022-08-08 20:50:11'
tags:
- ''
title: 青岛市2022编程市赛 T3结论与简易证明
updated: '2022-08-08 20:50:11'
---首先，结论与证明来自[http://oeis.org/A005732/a005732.pdf](http://oeis.org/A005732/a005732.pdf)，这里给出简单翻译和加工

给出结论，在一个圆上取 $n$ 个点相连，构成的三角形个数为 $C_{n+3}^6+C_{n+1}^5+C_n^5$

如何证明呢，我们不妨从圆上n个点中的任意个可以构成几个三角形来入手，我们先看7个点的图，来观察一下! ![pic1](https://cdn.staticaly.com/gh/JesseJeson/picture-api@master/20220808/p1.eha92tx5hhs.png)

如果你有**十足**的耐心的话，你可以数出来，这是 $287$ 个三角形

但是，我们要求出一个通项公式，我们可以发现，每一个三角形都是由多个或 $0$ 个圆上的点组成的，让我们通过这个分类讨论

首先tips：认识到圆“上"的概念：就是在圆的边上

1. 由圆上 $3$ 个点构成的三角形有 $C_n^3$ 个。证明：圆上每三个点可以构成一个三角形，反之亦然.（这个不用画图了吧qwq）
2. **只**由圆上 $2$ 个点 和另外 $1$ 个点 构成的三角形个数有 $4 \times C_n^4$ 个。证明：对于圆上的每 $4$ 个点，可以构成 $4$ 个"有 $2$ 个点在圆上" 的三角形  ![p2](https://cdn.staticaly.com/gh/JesseJeson/picture-api@master/20220808/p2.79xiq53ffrs0.png)
3. **只**由圆上 $1$ 个点 和另外 $2$ 个点 构成的三角形个数有 $5 \times C_n^5$个。证明：对于圆上的每 $5$ 点，可以构成 $5$ 个"有 $1$ 个点在圆上" 的三角形  ![p3](https://cdn.staticaly.com/gh/JesseJeson/picture-api@master/20220808/p3.djwlibx0skw.png)
4. **只**由圆上 $0$ 个点 和另外 $3$ 个点 构成的三角形个数有 $C_n^6$个。证明：对于圆上的每 $6$ 点，可以构成 $1$ 个"有 $0$ 个点在圆上" 的三角形  ![p4](https://cdn.staticaly.com/gh/JesseJeson/picture-api@master/20220808/p4.19ep3c9lu33.png)（当然，这个图里还有别点之间的的连线，只不过我懒得画了emmmm）

最后，就是整理这个式子了（挺复杂的qwq）（参考[this](https://blog.imoier.xyz/posts/13240/)）
$\because C_{n+1}^m = C_{n}^m + C_{n}^{m-1}$

$\therefore C_n^3 + 4 \times C_n^4 + 5 \times C_n^5 + C_n^6$

=$C_n^3 + 4 \times C_n^4 + 4 \times C_n^5 + C_n^5 + C_n^6$

=$C_n^3 + 4 \times C_{n+1}^5 + C_{n+1}^6$

=$C_n^3 + 3 \times C_{n+1}^5 + C_{n+1}^5 + C_{n+1}^6$

=$C_n^3 + 3 \times C_{n+1}^5 + C_{n+2}^6$

=$C_n^3 + C_{n+1}^5 + 2 \times C_{n+1}^5 + C_{n+2}^6$

=$C_n^3 + C_n^4 + C_n^5 + 2 \times C_{n+1}^5 + C_{n+2}^6$

=$C_{n+1}^4 + C_n^5 + 2 \times C_{n+1}^5 + C_{n+2}^6$

=$C_{n+2}^5 + C_{n+2}^6+C_n^5+C_{n+1}^5$

=$C_{n+3}^6 + C_n^5+C_{n+1}^5$
