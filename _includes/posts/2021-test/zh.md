---
layout:     post
title:      "图论MST-基于贪心+并查集的Kruscal算法"
subtitle:   "图上最小生成树Kruscal算法课堂笔记"
date:       2021-07-31 11:12:00
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
  - 最小生成树
  - Kruscal
  - 并查集
  - 联通块
---
# 算法简述

**Kruscal算法**，就是基于并查集的贪心算法，时间复杂度为mlogm（m为图的边数），用于解决**稀疏图**上的MST问题

流程：

- 首先，我们把所有的边按照权值从小到大排列，并且每个点属于不同的集合
- 将边从小到大遍历，如果这条边的两个端点不属于同一集合，那么就将它们合并，加入到最小生成树里
- 直到所有的点都属于同一个集合为止

集合自然可以使用并查集实现

一般来说，将找到权重最小的边可以用快速排序写

# 代码实现

```c++
int MST_Kruscal() {
	int ans = 0;
	sort(a+1, a+1+m);
	for(int i=1; i<=m; i++) {
		if(!same(a[i].u, a[i].v)) {
			unit(a[i].u, a[i].v);
			ans += a[i].w;
		}
	}
	return ans;
}
```

