# 《剑指offer》刷题笔记（知识迁移能力）：平衡二叉树

----------

- **转载请注明作者和出处：http://blog.csdn.net/u011475210**
- **代码地址：https://github.com/WordZzzz/Note/tree/master/AtOffer**
- **刷题平台：https://www.nowcoder.com/**
- **题&emsp;&emsp;库：剑指offer**
- **编&emsp;&emsp;者：WordZzzz**

----------

[toc]

## 题目描述

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

## 解题思路

可以在之前求二叉树深度的基础上进行算法实现，但是需要多次遍历。我们也可以用后序遍历，这样我们只需要遍历一次即可，需要需要存储一下左右子树的深度。

## C++版代码实现

### 多次遍历

```
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(pRoot == NULL)
            return true;
        int left = getDepth(pRoot->left);
        int right = getDepth(pRoot->right);
        int diff = left - right;
        if(diff > 1 || diff < -1)
            return false;
        return IsBalanced_Solution(pRoot->left) && IsBalanced_Solution(pRoot->right);
    }
    int getDepth(TreeNode* pRoot){
        return pRoot ? max(getDepth(pRoot->left),getDepth(pRoot->right))+1 :0;
    }
};
```

### 一次遍历

```
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;
        return IsBalanced(pRoot, &depth);
    }
    int IsBalanced(TreeNode* pRoot, int* depth){
        if(pRoot == NULL){
            *depth = 0;
            return true;
        }
        int left, right;
        if(IsBalanced(pRoot->left, &left) && IsBalanced(pRoot->right, &right)){
            int diff = left - right;
            if(diff <= 1 && diff >= -1){
                *depth = 1 + (left > right ? left : right);
                return true;
            }
        }
        return false;
    }
};
```

## Python版代码实现

### 多次遍历

```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def IsBalanced_Solution(self, pRoot):
        # write code here
        if pRoot == None:
            return True
        left = self.getDepth(pRoot.left)
        right = self.getDepth(pRoot.right)
        diff = abs(left - right)
        if diff > 1:
            return False
        return self.IsBalanced_Solution(pRoot.left) and self.IsBalanced_Solution(pRoot.right)
    def getDepth(self, pRoot):
        if not pRoot:
            return 0
        return max(self.getDepth(pRoot.left),self.getDepth(pRoot.right))+1
```

**<font color="red" size=3 face="仿宋">系列教程持续发布中，欢迎订阅、关注、收藏、评论、点赞哦～～(￣▽￣～)～</font>**

**<font color="red" size=3 face="仿宋">完的汪(∪｡∪)｡｡｡zzz</font>**