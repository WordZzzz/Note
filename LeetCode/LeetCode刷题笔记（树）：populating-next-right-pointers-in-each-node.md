---
title: LeetCode刷题笔记（树）：populating-next-right-pointers-in-each-node
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

Given a binary tree

    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set toNULL.

Initially, all next pointers are set toNULL.

Note:

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,
Given the following perfect binary tree,

         1
       /  \
      2    3
     / \  / \
    4  5  6  7

After calling your function, the tree should look like:

         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL

填充每个下一个指针指向其下一个右侧节点。如果没有下一个正确的节点，则应将下一个指针设置为NULL。 最初，所有下一个指针都设置为NULL。 注意： 您只能使用恒定的额外空间。 你可以假设它是一棵完美的二叉树（即所有叶子都在同一水平上，并且每个父母都有两个孩子）。

## 解题思路

递归的代码比较简单，思路也比较清晰，就是当前结点的左右孩子都不为空，则左孩子的next结点指向右孩子；如果当前结点的next结点和右孩子都不为空，则右孩子的next指向当前节点的next结点的左孩子（看例子中的第二三层更轻松）。

## C++版代码实现

```c
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(root == NULL)
            return;
        if(root->left != NULL && root->right != NULL)
            root->left->next = root->right;
        if(root->next != NULL && root->right != NULL)
            root->right->next = root->next->left;
        connect(root->left);
        connect(root->right);
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**