---
title: LeetCode刷题笔记（树）：binary-tree-maximum-path-sum
comments: true
mathjax: true
categories:
  - Leetcode
tags:
  - Leetcode
  - Tree
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

Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.

For example:
Given the below binary tree,

       1
      / \
     2   3

Return6.

给定一个二叉树，找到最大路径和。 路径可以在树中的任何节点开始和结束。 例如：给定下面的二叉树，   
       1
      / \
     2   3

返回6。

## 解题思路

递归的思想，先分别计算左右子树的最大值，然后通过比较maxValue和root->val + leftMax + rightMax来确定是否更行maxValue。最后，返回当前root的值加上其左右子树中的最大值，或者0。


## C++版代码实现


```c
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxValue = 0;
    int maxPathSum(TreeNode *root) {
        if(root == NULL)
            return 0;
        maxValue = -0x7fffff;
        getMaxPathSum(root);
        return maxValue;
    }
    
    int getMaxPathSum(TreeNode *root){
        if(root == NULL)
            return 0;
        int leftMax = max(0, getMaxPathSum(root->left));
        int rightMax = max(0, getMaxPathSum(root->right));
        maxValue = max(maxValue, root->val + leftMax + rightMax);
        return max(0, root->val + max(leftMax, rightMax));
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**