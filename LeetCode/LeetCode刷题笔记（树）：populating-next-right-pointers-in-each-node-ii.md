---
title: LeetCode刷题笔记（树）：populating-next-right-pointers-in-each-node-ii
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

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.

For example,
Given the following binary tree,

         1
       /  \
      2    3
     / \    \
    4   5    7

After calling your function, the tree should look like:

         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL

跟进问题“填充每个节点中右下角的指针”。 如果给定的树可以是任何二叉树，该怎么办？你以前的解决方案仍然有效吗？ 注意： 您只能使用恒定的额外空间。

## 解题思路

核心解题思路是，如果当前层所有结点的next 指针已经设置好了，那么下一层所有结点的next指针 也可以依次被设置。。其中dump->next每次指向的是下一行的最左结点。

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
        while(root){
            TreeLinkNode *dump = new TreeLinkNode(0);
            auto cur = dump;
            while(root){
                if(root->left){
                    cur->next = root->left;
                    cur = cur->next;
                }
                if(root->right){
                    cur->next = root->right;
                    cur = cur->next;
                }
                root = root->next;
            }
            root = dump->next;
        }
    }
};
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**