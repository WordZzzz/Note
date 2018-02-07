---
title: LeetCode刷题笔记（树）：minimum-depth-of-binary-tree
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

Given a binary tree, find its minimum depth.The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

给定一棵二叉树，找到它的最小深度。最小深度是沿着从根节点到最近叶节点的最短路径的节点数量。

## 解题思路

DFS和BFS都可以，但是很明显，BFS更快，因为BFS只要找到第一个叶子结点就可以停止遍历了看，而DFS需要通过迭代来遍历所有的结点。

这里我重点说一下BFS，循环的终止条件是now结点无左右子树，即size前后无变化。

## C++版代码实现

### 深度优先搜索

```c
class Solution {
public:
    int run(TreeNode *root) {
        if(root == NULL)
            return false;
        if(root->left == NULL)
            return run(root->right) + 1;
        if(root->right == NULL)
            return run(root->left) + 1;
        int leftDepth = run(root->left);
        int rightDepth = run(root->right);
        return min(leftDepth, rightDepth) + 1;
    }
};
```

### 广度优先搜索

```c
class Solution {
public:
    int run(TreeNode *root) {
        if(root == NULL)
            return false;
        queue<TreeNode *> que;
        TreeNode *last, *now;
        int level = 1, size = 0;
        last = now = root;
        que.push(root);
        while(!que.empty()){
            now = que.front();
            que.pop();
            size = que.size();
            if(now->left)
                que.push(now->left);
            if(now->right)
                que.push(now->right);
            if(que.size() == size)  //循环终止条件
                break;
            if(last == now){
                level++;
                if(que.size() != 0)
                    last = que.back();
            }
        }
        return level;
    }
};
```
**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**