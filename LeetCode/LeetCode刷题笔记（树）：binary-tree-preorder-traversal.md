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

两种方法，递归和迭代，递归太简单了，直接贴出代码就不多说了。

前序遍历需要注意的是，因为stack是后入先出规则，所以我们在入栈的时候，先将右子树入栈，再将坐子树入栈，这样在出栈的时候就可以保证是先左后右了。


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
    
    void preorder(TreeNode *root, vector<int> &res){
        if(root){
            res.push_back(root->val);
            preorder(root->left, res);
            preorder(root->right, res);
        }
    }
    
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> res;
        preorder(root, res);
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
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> res;
        if(root == NULL)
            return res;
        stack<TreeNode *> sta;
        sta.push(root);
        while(!sta.empty()){
            TreeNode *cur = sta.top();
            sta.pop();
            res.push_back(cur->val);
            if(cur->right)
                sta.push(cur->right);
            if(cur->left)
                sta.push(cur->left);
        }
        return res;
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**