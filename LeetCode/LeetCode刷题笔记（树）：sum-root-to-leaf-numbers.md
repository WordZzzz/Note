---
title: LeetCode刷题笔记（树）：sum-root-to-leaf-numbers
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

Given a binary tree containing digits from0-9only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path1->2->3which represents the number123.

Find the total sum of all root-to-leaf numbers.

For example,

    1
   / \
  2   3

The root-to-leaf path1->2represents the number12.
The root-to-leaf path1->3represents the number13.

Return the sum = 12 + 13 =25.

给定一个二进制树，只包含0-9的数字，每个根到叶的路径可以代表一个数字。 一个例子是代表数字123的根到叶子路径1-> 2-> 3。 找到所有根到叶子数量的总和。 例如，1 / \ 2 3根到叶子路径1 - > 2表示数字12。 根到叶子路径1-> 3表示数字13。 返回总和= 12 + 13 = 25。

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