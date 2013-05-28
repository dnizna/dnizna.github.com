---
layout: post
title: "并查集"
date: 2013-05-26 11:01
comments: true
categories: algorithm
---

### 概述 
并查集也叫不相交集合。他是一种树形结构，用来处理不相交集合的合并及查询问题。 

### 原理

#### 1. 不相交集合的根
并查集的集合内部存储是使用树形结构, 同一个集合里面的元素同属于一棵树。  
我们使用数组parent[x]表示节点x的父亲节点，体现节点之间这种树形的关系。如果节点x是这颗树的根，那么parent[x] = x;  

```java
void root(int x)
{
  while(parent[x] != x) x = parent[x];
  return x;
}
```

#### 2. 合并俩个不相交集合
如果节点x和节点y分属于俩颗树，也就是属于俩个不同集合。我们分别找到x, y这俩个节点所在树的根rootx, rooty。
将 parent[rootx] 指向 rooty，即 parent[rootx] = rooty。就可以将这俩颗树合并成一颗树了。  

```java
void union(int x, int y)
{
  int rootx = root(x), rooty = root(y);
  if(rootx != rooty)
  {
    parent[rootx] = rooty;
  }
}
```
 
#### 3. 查询
判断俩个元素是否在同一个集合只需判断他们所在树的根节点是否相同。

```java
int query(int x, int y)
{
  int rootx = root(x), rooty = root(y);
  return rootx == rooty;
}
```

#### 4. 路径压缩
在查找节点x所在的树的根的过程中，基本算法是一直找节点x的父亲节点。在这个
过程中，可以将这个链路上的所有节点的父亲节点直接指向根节点。
如: a->b->c->d 这样的链路，查找a的根节点过程中会遍历a,b,c节点，可以将a,b,c节点的父亲节点都直接
指向d，变成 a->d, b->d, c->d。下次查找过程就可以节省时间了。

```java
int root(int x)
{
  int rt = x;
  if(x != parent[x])
  {
    // 递归查找根节点
    rt = root(parent[x]);
    // 路径压缩，将链路上的节点的父亲节点直接指向根节点
    parent[x] = rt;
  }
  return rt;
}
```

### 应用

#### 1. 带权并查集
代码参考：<a href="https://github.com/dnizna/algorithm/blob/master/disjointset/poj1182.cpp" target="_blank">poj1182</a>

#### 2. 并查集的删除
代码参考：<a href="https://github.com/dnizna/algorithm/blob/master/disjointset/hdu2473.cpp" target="_blank">hdu2473</a>


