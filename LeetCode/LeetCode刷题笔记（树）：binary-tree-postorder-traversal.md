---
title: LeetCode刷题笔记（树）：binary-tree-postorder-traversal
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

Given a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree{1,#,2,3},

   1
    \
     2
    /
   3

return[3,2,1].

Note: Recursive solution is trivial, could you do it iteratively?

给定一个二叉树，返回其节点值的后序遍历。 

注意：递归解决方案是微不足道的，你可以迭代地做？

## 解题思路

两种方法，递归和迭代，递归太简单了，直接贴出代码就不多说了。

后序遍历的非递归版可以说是三种遍历中最难的了，因为要先穷举左右结点，最后才是根结点。在外循环中，第一步还是依次将左子树入栈；然后看栈顶元素，如果栈顶元素没有右孩子，或者右孩子时候已经遍历过，那么就弹出栈顶元素，并记录；否则，遍历右子树。


## C++版代码实现

### 递归

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
    
    void postorder(TreeNode *root, vector<int> &res){
        if(root){
            postorder(root->left, res);
            postorder(root->right, res);
            res.push_back(root->val);
        }
    }
    
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        postorder(root, res);
        return res;
    }
};
```

### 迭代

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
    vector<int> postorderTraversal(TreeNode *root) {
        vector<int> res;
        stack<TreeNode *> sta;
        TreeNode *cur = root, *last = NULL;
        while(cur != NULL || !sta.empty()){
            while(cur != NULL){
                sta.push(cur);
                cur = cur->left;
            }
            cur = sta.top();
            if(cur->right == NULL || cur->right == last){
                sta.pop();
                res.push_back(cur->val);
                last = cur;
                cur = NULL;
            }else
                cur = cur->right;
        }
        return res;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**