---
title: LeetCode刷题笔记（树）：binary-tree-preorder-traversal
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

Given a binary tree, return the preorder traversal of its nodes' values.

For example:
Given binary tree{1,#,2,3},

   1
    \
     2
    /
   3

return[1,2,3].

Note: Recursive solution is trivial, could you do it iteratively?

给定一个二叉树，返回其节点值的前序遍历。 

注意：递归解决方案是微不足道的，你可以迭代地做？

## 解题思路

采用先序遍历的思想(根左右)+数字求和的思想(每一层都比上层和*10+当前根节点的值)。


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
    
    int preorder(TreeNode *root, int sum){
        if(root == NULL)
            return 0;
        sum = sum * 10 + root->val;
        if(root->left == NULL && root->right == NULL)
            return sum;
        return preorder(root->left, sum) + preorder(root->right, sum);
    }
    int sumNumbers(TreeNode *root) {
        if(root == NULL)
            return 0;
        int sum = 0;
        return preorder(root, sum);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**