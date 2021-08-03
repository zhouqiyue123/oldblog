---
layout:     post
title:      "图论最短路-Bellman-Ford最短路径算法"
subtitle:   "图上最短路Bellman-Ford算法课堂笔记"
date:       2021-08-01 19:02:00
author:     "周琪岳"
header-img: "img/post-bg-css.jpg"
header-img-credit: "@WebdesignerDepot"
header-img-credit-href: "medium.com/@WebdesignerDepot/poll-should-css-become-more-like-a-programming-language-c74eb26a4270"
header-mask: 0.4
catalog: true
tags: 
  - C++
  - 数据结构
  - 图论
  - 最短路
  - Bellman-Ford

---

# 算法简述

**Bellman-Ford算法**，用于解决不包含负权重环的图上单源最短路径问题。时间复杂度为**O(n*m)**。时间复杂度比Dijkstra高，但是可以检测负环。

### 算法流程

 松弛迭代n-1轮

​	每一轮对所有的边进行 [^松弛]操作

经过n-1轮 所有点都得到了最短路径

原因：因为最短路最多包含n-1条边，经过n-1轮的迭代，顶点要么已经求出最短路，要么与原点不联通

如果再进行一轮，可以进行松弛操作，则必然存在负环

# 代码实现

```c++
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

const int MAXN = 1e2 + 100, MAXM = 1e4 + 100;
int n, m;
struct edge {
	int next, w;
};
vector<edge> g[MAXM];
int ans;

int dis[MAXN];
bool Ford(int s) {
	memset(dis, 0x3f, sizeof(dis));
	dis[s] = 0;
	for(int i=1; i<n; i++) { // n-1轮松弛操作 
		for(int j=1; j<=n; j++) { // 遍历所有点 
			for(int k=0; k<g[j].size(); k++) {
				edge e = g[j][k];
				if(dis[e.next] > dis[j] + e.w) {
					dis[e.next] = dis[j] + e.w; 
				}
			}
		}
	}
	for(int j=1; j<=n; j++) { // 遍历所有点 
		for(int k=0; k<g[j].size(); k++) {
			edge e = g[j][k];
			if(dis[e.next] > dis[j] + e.w) {
				return false;
			}
		}
	}
}

int main() {
	cin >> n >> m;
	for(int i=1, u, v, w; i<=m; i++) {
		cin >> u >> v >> w;
		g[u].push_back((edge){v, w});
		g[v].push_back((edge){u, w}); 
	}
	Ford(1);
	for(int i=1; i<=n; i++) {
		if(dis[i] == 0x3f3f3f3f) {
			cout << -1 << " ";
		} else {
			cout << dis[i] << " ";
		}
	}
	return 0;
}
```

[^松弛]: 即为对于图上当前点，尝试更新最短路径

