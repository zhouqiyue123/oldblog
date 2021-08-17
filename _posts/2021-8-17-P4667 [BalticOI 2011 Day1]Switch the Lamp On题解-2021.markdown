---
layout:     post
title:      "P4667 [BalticOI 2011 Day1]Switch the Lamp On题解"
subtitle:   "Dij最短路"
date:       2021-08-17 14:35:00
author:     "周琪岳"
header-style: text
catalog: false
tags: 
  - C++
  - 洛谷
  - 题解
  - Dijkstra
  - 图论
  - 最短路
---

## 初步分析

读完题，题目要求求最短路，又是电路图，大概率是bfs或者最短路的一堆算法。如果用bfs的话，每扩展一个新节点，所需增加的花费不一致（因为有可能需要转动电路），而本蒟蒻不会双端队列实现广搜，所以只会用dijkstra求解。

## 思路成形

#### 建图

最短路的前提条件是建图。我们可以将每个交点当做图上顶点，电路通道就是花费为0的边，实际上出了标出的电路通道，还有“隐藏的通道”，就是通过翻转电路所形成的通道，只不过边权为1。

#### 最短路

建完图后，只需要对整个点矩阵求最短路就可以了。我是从(0, 0)开始标号到(n,m)的，直接用迪杰斯特拉求出最短路径就行了（别用SPFA 它死了 容易被卡 反正边权非负）只不过新顶点入队有点麻烦，仔细判一下就行了（我最开始有个条件写反了，100->30）

#### 时间复杂度

建图肯定用不了多少时间，dijkstra的复杂度大约是O(n · m · $\log_2nm$)，为正解

## 代码实现

```c++
#include <iostream>
#include <cstring>
#include <queue>
#define OUTPUT "NO SOLUTION"

using namespace std;

const int MAXN = 505;
const int dir[4][2] = {
	{1, 1}, {1, -1}, {-1, -1}, {-1, 1}
};
int n, m;
bool a[MAXN][MAXN];
/* 0表示/ */ 
/* 1表示\ */
struct node {
	int dis, x, y;
	bool operator > (const node &T) const {
		return dis > T.dis;
	}
};
priority_queue<node, vector<node>, greater<node> > q;
int dis[MAXN][MAXN];

bool valid(int x, int y) {
	return x >= 0 && x <= n && y >= 0 && y <= m;
}

void Dijkstra(int sx, int sy) {
	memset(dis, 0x3f, sizeof(dis));
	dis[sx][sy] = 0;
	q.push((node){dis[sx][sy], sx, sy});
	while(!q.empty()) {
		node t = q.top(); q.pop();
		
		if(dis[t.x][t.y] < t.dis) continue;
		dis[t.x][t.y] = t.dis;
		// cout << t.x << " " << t.y << " " << t.dis << endl;
		for(int i=0; i<4; i++) {
			int nx = t.x + dir[i][0], ny = t.y + dir[i][1];
			// cout << "	" << nx << " " << ny << endl;
			if(!valid(nx, ny)) continue;
			/* 
			无需转动的条件： 
				i==0 右下： a [t.x+1][t.y+1]   反斜杠
				i==1 左下： a [t.x+1]  [t.y]   正斜杠
				i==2 左上： a [t.x][t.y]     反斜杠
				i==3 右上： a [t.x]  [t.y+1]     正斜杠 
			*/
			if( (i == 0 && a[t.x+1][t.y+1] || i==1 && !a[t.x+1][t.y] || i==2 && a[t.x][t.y] || i==3 && !a[t.x][t.y+1] )
				&& dis[nx][ny] > dis[t.x][t.y] ) {
				dis[nx][ny] = dis[t.x][t.y];
				q.push((node){dis[nx][ny], nx, ny});
			} else if(dis[nx][ny] > dis[t.x][t.y] + 1) {
				dis[nx][ny] = dis[t.x][t.y] + 1;
				q.push((node){dis[nx][ny], nx, ny});
			}
		}
	}
}

int main() {
	cin >> n >> m;
	for(int i=1; i<=n; i++) {
		for(int j=1; j<=m; j++) {
			char c;
			cin >> c;
			if(c == '/') a[i][j] = 0;
			else a[i][j] = 1;
		}
	}
	Dijkstra(0, 0);
	if(dis[n][m] == 0x3f3f3f3f) cout << OUTPUT;
	else cout << dis[n][m];
	return 0;
}
```



，
