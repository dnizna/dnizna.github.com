---
layout: post
title: "树状数组"
date: 2013-05-26 10:39
comments: true
categories: algorithm
---

### 概念
对于一个整形数组A[n]， 动态修改数组中的项A[i]，然求A[0]到A[j]的和。这种问题传统的做法需要的时间复杂度是O(n)。 树状数组是一种数据结构可以使用O($$log_{2}n$$)的时间在线求数组和。

### 原理

#### 1. lowbit函数
对整数 x 表示成二进制之后，求出二进制表示中最右边的 1 的位置。如:  

```java
 lowbit(6)=lowbit(0b110)=0b10=2
 lowbit(5)=lowbit(0b101)=0b1=1
 lowbit(4)=lowbit(0b100)=0b100=4
 lowbit(3)=lowbit(0b11)=0b1=1
```

奇数的 lowbit 函数值都为 1 。lowbit 的函数值都是 2 的 n 次方。   
使用位运算可以很容易求出这个值:   

```c
#define lowbit(x)  (-(x)&(x))
```

负数的二进制表示是其正数的反码加上1。如果一个数是 2 的 n 次方，这个数的正负数的二进制表示是相同的，只是符号位不同。  

记 S[x] 表示A[n]数组中 A[$$x−2^p+1$$] 到 A[x] 中这一段的和。即：  
S[$$x$$] = A[$$x−2^p+1$$] + A[$$x−2^p+2$$] + … + A[$$x$$]，其中 $$2^p$$ 的值为 lowbit(x)。  

#### 2. 数组求和
对于 A[1] 到 A[x] 的和sum[x] 可以通过 S[x] 使用以下方式求得：  
sum[x] = S[x] + S[x - lowbit(x)] + S[x - lowbit(x) - lowbit(x - lowbit(x))] + … 直到下标为0。  

```java
// 树状数组计算 A[1] 到 A[x] 的和
int sum(x)
{
  int sum = 0;
  for(; x; x -= lowbit(x))
  {
    sum += S[x];
  }
  return sum;
}
```

#### 3. 数组更新
如果修改了数组A[N]中某一项的值A[x]，如何保证S[x]也同时进行更新。  
思考下，A[x]的值修改了，会影响到S[N]中哪些项的值。不难得到，对于S[N]数组中的某一项 S[y]，如果 y - lowbit(y) > x， 那么 S[y] 的值必然受到 A[x] 值修改的影响。所以更新A[x]的值时需要同时更新受影响的S[y]的值。

```java
// 树状数组更新值
void add(int x, int v)
{
  A[x] += v;
  for(; x < N; x+= lowbit(x))
  {
    S[x] += v;
  }
}
```

如x = 3， lowbit(x) = 1， x + lowbit(x) = 4。S[4] 表示 A[1]+ … + A[4]， 包含A[3]，故S[4] 需要更新。  
4 + lowbit(4) = 8。 S[8] 表示 A[1] + A[2] + … + A[8] 的和，包含 A[3]，故S[8]需要更新。  

### 应用

#### 1. 求逆序数

a. 对数据进行离散化。
所谓离散化就是将大的数据映射到小的数据。如一个数组包含[10000, 10, 1000, 100]四个数，因为逆序只关心数据的大小关系， 并不关系数据的值到底是多少。这个数组的逆序数的个数和数组[4,1,3,2]是完全相同的。建立这样的映射关系是减少数据的处理范围。

b. 按照数组的顺序依次将离散化后的数据插入到树状数组。 
插入离散化后的数据x前，统计当前状态中包含多少个小于x的数。然后将当前状态 的数据总量减去这个数就是大于x的数的数量，也就是当前状态中和x这个数据匹配的逆序的数量。

代码参考：<a href="https://github.com/dnizna/algorithm/blob/master/treearray/poj2299.cpp" target="_blank">poj2299</a>

#### 2. 二维树状数组以及多维

```java
void add(int x, int y, int v)
{
  for(int i = x; i < N; i += lowbit(i))
  {
    for(int j = y; j < N; j += lowbit(j))
    {
      S[i][j] += v;
    }
   }
}

int sum_(int x, int y)
{
  int s_ = 0;
  for(int i = x; i; i-= lowbit(i))
  {
    for(int j = y; j; j-= lowbit(j))
    {
      s_ += S[i][j];
    }
  }
  return s_;
}
```

代码参考：<a href="https://github.com/dnizna/algorithm/blob/master/treearray/hdu1829.cpp" target="_blank">hdu1829</a>
