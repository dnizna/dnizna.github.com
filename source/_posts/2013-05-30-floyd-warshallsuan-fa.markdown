---
layout: post
title: "floyd-warshall算法"
date: 2013-05-30 14:49
comments: true
categories: algorithm
---

### 基本概念
floyd-warshall算法用来求图论中的任意俩点之间的最短路径。

### 原理
floyd-warshall算法的原理是动态规划。  
我们使用 dist[i,j,k] 表示顶点 i 和顶点 j 的路径中经过的顶点的编号不大于 k  的最短路径。  
不难得到以下递推式：  
$$dist[i,j,k] = min( dist[i, j, k], dist[i, k, k-1] + dist[k, j, k-1] )$$

用代码实现为：  

```cpp
for(int k = 1; k <= n; ++k)
  for(int i = 1; i <= n; ++i)
    for(int j = 1; j <= n; ++j)
      if(dist[i][k][k-1] + dist[k][j][k-1] < dist[i][j][k])
        dist[i][j][k] = dist[i][k][k-1] + dist[k][j][k-1];
```

实际中，空间可以减少到二维:  

```cpp
for(int k = 1; k <= n; ++k)
  for(int i = 1; i <= n; ++i)
    for(int j = 1; j <= n; ++j)
      if(dist[i][k] + dist[k][j] < dist[i][j])
        dist[i][j] = dist[i][k] + dist[k][j];
```

### 应用

#### 基本使用
<a href="https://github.com/dnizna/algorithm/blob/master/shortestpath/zoj1082.py" target="_blank">zoj1082</a>

#### 求最小环
根据递推式的意义， dist[i][j][k] 表示顶点 i 和顶点 j 的路径中经过的顶点编号不大于 k 的最短路径。  
在经过 i, j 路径中加入顶点 k 前，dist数组记录了 i 和 j 路径中经过的顶点编号小于k的最短路径，假设顶点 k 在
最小环中，这时环的长度为 dist[i][j][k-1] + mat[i][k] + mat[k][j]。  mat[k][j] 表示顶点 k 和顶点 j 的长度。 
我们更新这个环长度的最小值即可。

<a href="https://github.com/dnizna/algorithm/blob/master/shortestpath/hdu1599.cpp" target="_blank">hdu1599</a>


