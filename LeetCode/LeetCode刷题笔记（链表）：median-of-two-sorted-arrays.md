---
title: LeetCode刷题笔记（链表）：median-of-two-sorted-arrays
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - List
---

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/LeetCode**
- **刷题平台：https://www.nowcoder.com/ta/leetcode**
- **题&emsp;&emsp;库：Leetcode经典编程题**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

There are two sorted arrays A and B of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

## 解题思路

要求在O(log (m+n))时间内找到中位数，所以像那些合并之后再二分查找、或者一边比较一边合并到总量一半的方法肯定是不行的。

我们可以将原问题转变成一个寻找第k小数的问题（假设两个原序列升序排列），这样中位数实际上是第(m+n)/2小的数。所以只要解决了第k小数的问题，原问题也得以解决。

首先假设数组A和B的元素个数都大于k/2，我们比较A[k/2-1]和B[k/2-1]两个元素，这两个元素分别表示A的第k/2小的元素和B的第k/2小的元素。这两个元素比较共有三种情况：>、<和=。如果A[k/2-1] < B[k/2-1]，这表示A[0]到A[k/2-1]的元素都在A和B合并之后的前k小的元素中。换句话说，A[k/2-1]不可能大于两数组合并之后的第k小值，所以我们可以将其抛弃。

当A[k/2-1]=B[k/2-1]时，我们已经找到了第k小的数，也即这个相等的元素，我们将其记为m。由于在A和B中分别有k/2-1个元素小于m，所以m即是第k小的数。(这里可能有人会有疑问，如果k为奇数，则m不是中位数。这里是进行了理想化考虑，在实际代码中略有不同，是先求k/2，然后利用k-k/2获得另一个数。)

通过上面的分析，我们即可以采用递归的方式实现寻找第k小的数。此外我们还需要考虑几个边界条件：

- 如果A或者B为空，则直接返回B[k-1]或者A[k-1]；
- 如果k为1，我们只需要返回A[0]和B[0]中的较小值；
- 如果A[k/2-1]=B[k/2-1]，返回其中一个；

## C++版代码实现

```c
class Solution {
public:
    
    double findKth(int A[], int m, int B[], int n, int k){
        if(m > n)
            return findKth(B, n, A, m, k);  //始终保持元素较少的数组位于前面的位置
        if(m == 0)
            return B[k-1];                  //如果位于前面的数组为空，则直接返回后面数组的第k-1个元素
        if(k == 1)
            return min(A[0], B[0]);         //如果k等于1，则返回两个数组头元素的最小值
        
        int pa = min(k / 2, m), pb = k - pa;
        if(A[pa-1] < B[pb-1])
            return findKth(A + pa, m - pa, B, n, k - pa);
        else if(A[pa - 1] > B[pb - 1])
            return findKth(A, m, B + pb, n - pb, k - pb);
        else
            return A[pa-1];
    }
    
    double findMedianSortedArrays(int A[], int m, int B[], int n) {
        int total = m + n;
        if(total & 0x1)
            return findKth(A, m, B, n, total / 2 + 1);
        else
            return (findKth(A, m, B, n, total/2)
                   + findKth(A, m, B, n, total / 2 + 1)) / 2;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**