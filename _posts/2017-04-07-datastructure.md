---
layout: post
title:  "note of Data Structure"
image: ''
date:   2017-04-07 12:54:43
tags:
- coding for fun
description: ''
categories:
- Learn 
---

#数据结构的定义
数据结构是计算机存储，组织数据的方式。数据结构是指相互之间存在一种或多种特定关系的数据元素的结合。
#栈
括号匹配，表达式计算 
#字符串匹配算法
- KMP算法
next 与 nextval 的计算
如果j位置和k位置字符相等，则next[++j] = ++k;
不然跳回next[k]的位置在进行比较.
```
public static int[] getNext(String ps)
{
char[] strKey = ps.toCharArray();
int[] next = new int[strKey.length];

// 初始条件
int j = 0;
int k = -1;
next[0] = -1;

// 根据已知的前j位推测第j+1位
while (j < strKey.length - 1)
{
if (k == -1 || strKey[j] == strKey[k])
{
next[++j] = ++k;
}
else
{
k = next[k];
}
}

return next;
}
``` 
nextval数组的求解方法是：nextval[1]=0。从第二位开始，若要求nextval[i]，将next[i]的值对应的位的值与i的值进行比较（例如，第i为的值为'b'，next[i]=3,则将i的值'b'与第三位的值进行比较），若相等，nextval[i]=nextval【next[i]】（例，nextval[i]=nextval[3]）；若不相等，则nextval[i]=next[i]（例，nextval[i]=next[i]=3）。
```
int get_nextval(SString T,int &nextval[ ]){
//求模式串T的next函数修正值并存入数组nextval。
i=1; nextval[1]=0; j=0;
while(i<T[0]){
if(j==0||T[i]==T[j]){
++i;++j;
if (T[i]!=T[j]) nextval[i]=j;
else nextval[i]=nextval[j];
}
else j=nextval[j];
}	
}//get_nextval
```
#二叉树遍历
前序遍历：根节点先遍历，非递归方法：利用队列。
中后序遍历：略。
#堆
- 定义：可以看成一个完全二叉树，所有非终端节点不大于其左右节点的值；
- 插入：将元素加入数组尾，与所有父节点组成一个有序序列。
```
int HeapInsert(int *heap,int n,int num)
{
int i , j ;
heap[n] = num;
i = n;
j = (n-1) / 2; // 指向父节点；

while(j >= 0 && i != 0)
{
if(heap[j] <= num)
break;
heap[i] = heap[j];
i = j;
j = (i - 1)/2;
}
heap[i] = num;
return 0;
}
```
- 删除：只能删除堆顶元素
```
int HeapAdjust(int *heap,int top,int n)
{
int j = 2*top + 1; // zuo
int temp = heap[top];
while(j < n)
{
if(j + 1 < n && heap[j+1] < heap[j])
j++;
if(heap[j] >= temp)
break;
heap[top] = heap[j];
top = j;
j = 2 * top + 1;
}
heap[top] = temp;
return 0;
}
int HeapDelete(int *heap,int n)
{
heap[0] = heap[n-1];
HeapAdjust(heap,0,n-1);
return 0;
}
```
- 堆排序
原理：每次删除根节点，因为根节点是最小的，然后再重建树。
#哈希
- 哈希函数
1.直接定址法：H(key) = key || H(key) = a * key + b，线性法.
2.数字分析法：瞎比分析
3.平方取中法：取关键字平方后中间几位数作为哈希地址。
4.折叠法：将关键字分成位数相同的几部分，各部分进行与操作。
5.除留余数法：H(key) = key MOD p;最常用。
6.随机数法：H(key) = random(key)。
考虑的因素：
1.计算哈希函数所需要的时间。
2.关键字的长度。
3.哈希表的大小。
4.关键字的分布情况。
5.记录的查找频率。
- 处理冲突的方法：
1.开放定址法：
Hi = (H(key) + di) MOD m;其中m为哈希表长，di为增量序列，有下列三种取法
```
di = 1,2,3....m-1 线性探测再散列。
di = 1^2,-1^2,2^2,-2^2......   二次探测再散列
di = 伪随机数序列 伪随机再散列
```
2.链地址法：同义词保存在一个链表里。
3.再哈希法：Hi = RHi(key) 再用哈希函数计算一个新的哈希值。
4.建立一个公共溢出区：所有冲突的数据填入溢出表（溢出表的地址与哈希地址无关）。
#二叉搜索树
- 定义：左结点小于父节点，右节点大于父节点。
- 查找：若b是空树，失败，若x=b的根节点，成功，若x小于，找左子树，x大于找右子树。
- 插入：小于插入左子树，大于插入右子树。
- 删除：分情况讨论
```
若*p为叶子节点，直接删除即可。
若*p只有左子树或者只有右子树，直接删除即可。
若*p左子树右子树不空，在删除后依旧要保持位置不变。按中序遍历保持有序，即在右子树中寻找最小的一个节点，与要删除节点交换，然后删除该叶子节点。
```
#二叉平衡树
- 定义：左右子树深度之差不超过1
- 右旋：
```
void RotateRight(tNode **root)
{
tNode *p = *root;
tNode *rc = p->left;
tNode *rrc = p->left->right;
p->left = rrc;
rc->right = p;
*root = rc;
}
```
- 左旋：
```
void RotateLeft(tNode **root)
{
tNode *p = *root;
tNode *lc = p->right;
tNode *llc = lc->left;
p->right = llc;
lc->left = p;
*root = lc;
}
```
- 插入结点算法：
1.LL情况：插入的是左子树的左子树。只用右旋根节点即可。
2.LR情况：插入的是左子树的右子树。先左旋插入结点，然后按LL型处理。
3.RR情况：直接左旋。
4.RL情况：先右旋，然后按照RR处理。
#红黑树
定义：红黑树是弱平衡树，如果搜索次数多，应该选择AVL树，如果插入删除次数差不多，选择红黑树。
染色法：根节点是黑色，叶子节点是黑色，如果一个节点是红色，子节点是黑色的，从一个节点到该节点的子孙路径上包含相同数目的黑节点。
#图
- 图的存储方式
1.数组表示。
2.邻接表。
3.十字链表。
#B-树
- 定义：是一种平衡多路搜索树。
1.定义任意非叶子节点最多有M个儿子。
2.根节点的儿子数为[2,M]
3.除根节点外非叶子节点儿子数目为[M/2,M]
4.每个节点至少存放[m/2-1,m-1]个关键字。
- 特征：
1.关键字集合分布整棵树。
2.任何一个关键字只出现在一个结点中。
3.搜索可能在非叶子节点中结束。
4.效率相当于二分查找。
#B+树
- 定义：基本与B-树相同。
1.所有关键字在叶子结点中出现。
2.非叶子节点指针与关键字数目相同。
3.叶子节点向链表一样串起来。
- 特征：
1.只能在叶子节点中命中。
2.非叶子节点相当于稀疏索引。
3.所有关键字出现在叶子节点中。
4.适合文件系统。
#B*树
- 定义：基本与B+树相同。
1.在非叶子节点中添加指向兄弟节点的指针。
2.非叶子节点的使用率提高到2/3.
- 分裂操作：
如果兄弟节点已满，在两个节点之间新建一个节点，各复制1/3数据到新的节点。
