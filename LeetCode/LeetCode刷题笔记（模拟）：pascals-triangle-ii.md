---
title: LeetCode刷题笔记（模拟）：pascals-triangle-ii
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Simulation
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

Given an index k, return the k th row of the Pascal's triangle.

For example, given k = 3,
Return[1,3,3,1].

Note: 
Could you optimize your algorithm to use only O(k) extra space?

给定一个索引k，返回帕斯卡三角形的第k行。 例如，给定k = 3， 返回[1,3,3,1]。 注意： 你可以优化你的算法只使用O（K）额外的空间？

## 解题思路

基本的想法是从头到尾迭代地更新数组。

## C++版代码实现

```c
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> dummp(rowIndex+1, 1);
        for(int i = 1; i < rowIndex; ++i)
            for(int j = i; j > 0; --j)
                dummp[j] += dummp[j-1];
        return dummp;
    }
}
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**