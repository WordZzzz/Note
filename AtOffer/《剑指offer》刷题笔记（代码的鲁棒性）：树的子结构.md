# 《剑指offer》刷题笔记（代码的鲁棒性）：树的子结构

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/CodingInterviewChinese2**
- **文章地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 前言

之前在leetcode刷题的时候，感觉做的最多的就是树这块了。

## 题目描述

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

## 解题思路

第一步在树A中查找与根结点的值一样的结点，这实际上就是树的遍历。所以，递归和循环都可以。

第二步是判断树A中以R为根结点的子树是不是和树B有相同的结构。同样的，递归和循环都可以。

## C++版代码实现

### DFS

```c
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot1 == NULL || pRoot2 == NULL)
            return false;
        return dfs(pRoot1, pRoot2) || HasSubtree(pRoot1->left, pRoot2) || HasSubtree(pRoot1->right, pRoot2);
    }
private:
    bool dfs(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL)
            return true;
        if(pRoot1 == NULL)
            return false;
        return pRoot1->val == pRoot2->val && dfs(pRoot1->left, pRoot2->left) && dfs(pRoot1->right, pRoot2->right);
    }
};
```

## Python 代码实现

### DFS

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        if not pRoot1 or not pRoot2:
            return False
        return self.dfs(pRoot1, pRoot2) or self.HasSubtree(pRoot1.left, pRoot2) or self.HasSubtree(pRoot1.right, pRoot2)
    def dfs(self, pRoot1, pRoot2):
        if not pRoot2:
            return True
        if not pRoot1:
            return False
        return pRoot1.val == pRoot2.val and self.dfs(pRoot1.left, pRoot2.left) and self.dfs(pRoot1.right, pRoot2.right)
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**