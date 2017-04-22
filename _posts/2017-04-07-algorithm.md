---
layout: post
title:  "note of Algorithm"
image: ''
date:   2017-04-07 12:54:43
tags:
- coding for fun
description: ''
categories:
- Learn 
---

# 算法的特征
- 有穷性
- 确定性
- 输入输出
- 可行性
# 算法复杂度的定义
- 大O 渐进上界
- θ 同时满足渐进上界以及渐进下届
- Ω 是渐进下界
- 小o 非紧渐进上界
# 递归算法
- 定义
递归算法是把问题转化为规模缩小了的同类问题的子问题。然后递归调用函数（或过程）来表示问题的解。
一个过程(或函数)直接或间接调用自己本身,这种过程(或函数)叫递归过程(或函数).
- 要求
每次调用规模有所缩小
相邻两次重复之间有紧密的联系，前一次腰围后一次做准备。
在规模极小时直接给出答案而不进行递归。
- 两个要素
终止条件以及递推关系。
# 分治算法的思想
基本思想是将一个规模为n的问题分解为k个规模较小的子问题，这些字问题互相独立且与原问题相同。递归的解决这些字问题，然后将各子问题的解合并的到原问题的解。
# # 全排列
分治：将a[1...k]拆分为a[1...k-1]与a[k]；
解决：求a[1...k-1]的全排列
合并：将a[1...k-1]的全排列与a[k]组合；
# # 二分搜索
分治：将a[1...k]拆分为相同的两半,a[1...k/2],a[k/2...k]；
解决：与中间的元素进行对比，小于则在左边寻找，大于在右边寻找
合并：
# # 归并排序
分治：将待排元素分成大小大致相同的两个子集合
解决：分别对两个子集合进行排序
合并：合并子集合
# # 快速排序
分治：将待排元素分成大小大致相同的两个集合
解决：将比k大的元素放在k右边，将比k小的放在k左边
合并：
代码：
```
void Qsort(int a[],int low,int high)
{
if(low >= high)
return;
int first = low;
int last = high;
int key = a[first];
while(first < last)
{
while(first < last && a[last] >= key)
{
last--;
}
a[first] = a[last];
while(first < last && a[first] <= key)
{
first--;
}
a[last] = a[first];
}
a[first] = key;
Qsort(a,low,first-1);
Qsort(a,first+1,high);
}
```
# # 线性时间选择
问题描述:给定线性序列集中n个元素和一个整数k，要求找出第k小的元素。
思想：快速排序思想
改进：将所有元素五个一组排序，找到中位数的中位数，当哨兵
# # 最接近点问题
问题描述：给定平面上n个点，找其中的一对点，使得n个点组成的所有点对中，该点对间的距离最小。
算法思想：将空间中的点分成左右两块，计算左右两块中的最短距离d，在每块的交界处计算x+d以及x-d中的点的最短距离，若d' < d，则d = d'；
# 动态规划
- 解题框架
将带求解的问题分解成若干个子问题，按照顺序求解子阶段，前一子问题的解，为后一子问题的求解提供了有用的信息。在求解任意子问题时，列出各种可能的菊不洁，通过决策保留那些有可能达到最优的局部解。
1.划分阶段：按照问题的空间或时间特征，把问题分解为若干阶段。
2.确定状态和状态变量：问题发展到不同阶段的不同状态无后效性。
3.确定决策并写出状态转移方程。
4.写出边界条件。
- 三要素
1.问题的阶段
2.每个阶段的状态
3.从前一个阶段转化到后一个阶段的递推关系。
- 备忘录方法
与动态规划不同，备忘录方法类似于递归，是一个自顶向下求解的过程。
# # 矩阵连乘问题
代码
```
int lookupChain(int i,int j)
{
if(m[i][j] > 0)
return m[i][j];
if(i == j)
return 0;
int u = lookupChain(i,i) + lookupChain(i+1,j) + p[i-1]*p[i]*p[j];
s[i][j] = i;
for(int k = i + 1;k < j;k++)
{
int t = lookupChain(i, k) + lookupChain(k+1, j) + p[i-1] * p[k] * p[j];
if(t < u){
s[i][j] = k;
u = t;
}
}
m[i][j] = u;
return u;
}
```
# # 最长公共子序列
解题思想：
1.若xm = yn，则zk = xm = yn 则 zk-1 是 xm-1 ,yn-1的最长公共子序列。
2.若xm != yn
3.若xm != yn

- 求长度
```
void LCSLength(int m,int n,char *x,char *y,int  **c,int **b)
{
int i,j;
for(i = 0;i<=m;i++)
c[i][0] = 0;
for(i = 0;i<=n;i++)
c[0][i] = 0;
for(i = 0;i<=m;i++)
{
for(j = 1;j<=n;j++)
{
if(x[i] == y[j])
{
c[i][j] = c[i-1][j-1] + 1;
b[i][j] = 1;
}
else if(c[i-1][j] >= c[i][j-1])
{
c[i][j] = c[i-1][j];
b[i][j] = 2;
}
else
{
c[i][j] = c[i][j-1];
b[i][j] = 3;
}
}
}
}
```
- 求序列
```
void LCS(int i,int j,char *x,int **b)
{
if(i == 0 || j == 0)
return;
if(b[i][j] == 1)
{
LCS(i-1,j-1,x,b);
cout << x[i];
}
else if(b[i][j] == 2)
{
LCS(i-1,j,x,b);
}
else
LCS(i,j-1,x,b);
}
```
# # 最大子段和
```
if(b > 0)
a[i] = b + a[i-1];
else
a[i] = a[i-1];
```
# # 01背包问题
# 贪心算法
- 贪心算法的要素
1.贪心选择性：所求问题的整体最优解可以通过一系列局部优化的选择，即贪心选择来达到。
2.最优子结构性质：当一个问题的最优解包含其子问题的最优解时，称此问题最有最优子结构性质。
# # 活动安排问题
算法思想：每次选择结束时间最早的且开始时间不冲突的活动即可。
# # 背包问题
算法思想：尽量选择价值大的物品。
# # 最优装载问题
算法思想：先装轻的。
# # 哈夫曼编码
# # 单源最短路径
```
for(int i = 1;i < n;i++)
{
int temp = maxint;
int u = v;
for(int j = 1;j <= n;i++){
if((!s[j])&&(dist[j]<temp))
{
u = j;
temp = dist[j];
}
s[u] = true;
for(int j = 1;j <=n ;j++)
{
if((!s[j])&&c[u][j] < maxint)
{
int newdist = dist[u] + c[u][j];
if(newdist < dist[j])
{
dist[j] = newdist;
prev[j] = u;
}
}
}
}
}
```
# # 最小生成树
- prim算法：一个点慢慢连接
- kruskal算法：两个点两个点的选择
